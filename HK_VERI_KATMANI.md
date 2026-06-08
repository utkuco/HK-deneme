# Veri Katmanı: Banka Bağlantısı & Kategorileme

> **Bağlı dökümanlar:** [`HK_BUTCE_YONETIMI.md`](./HK_BUTCE_YONETIMI.md), [`HK_ENTEGRASYON_DETAY.md`](./HK_ENTEGRASYON_DETAY.md)
> **Tarih:** 2026-06-08
> **Hazırlayan:** Seran (Claude aracılığıyla)

---

## Özet

Üç teknik karar:

1. **Banka bağlantısı:** 3. parti aggregator (AISP) ile başla, kademeli olarak top 5 bankaya direkt geç.
2. **Kategorileme:** Hibrit, kural öncelikli. MVP: MCC + lookup + regex. Faz 2: ML fallback.
3. **HK hesap entegrasyonu:** Mevcut HangiKredi kullanıcı hesabı = bütçe hesabı. Ayrı hesap yok.

Ortak prensip: **hızlı go-to-market + kontrollü evrim.** Önce çalışan bir şey, sonra optimize.

---

## 1. Banka Bağlantısı

### Seçenekler

| Yaklaşım | Time-to-prod | Maliyet | Operasyonel yük | Vendor riski |
|---|---|---|---|---|
| Direkt BDDK API (her banka) | 12-18 ay | Sadece dev | Yüksek (25+ banka, sertifika rotasyonu, API değişiklikleri) | Yok |
| 3. parti aggregator (AISP) | 1-2 ay | İşlem/kullanıcı başı komisyon | Düşük | Lock-in, downtime |
| Hibrit: aggregator + top 5 direkt | 2-3 ay başlangıç, 6-12 ay sonra direkt geçiş | Geçişle düşen komisyon | Orta | Yönetilebilir |

### Karar: 3. parti aggregator ile başla, kademeli direkte geç

**Gerekçe:**
- HK'nın asıl değeri ürün eşleştirme; banka API entegrasyonu commodity'ye yakın iş.
- 25+ bankaya ayrı entegrasyon ürün ekibini 1+ yıl boğar; her bankanın BDDK implementasyonu farklı, dokümantasyonu zayıf.
- Türkiye'de BDDK lisanslı AISP firmaları mevcut (Fincloud, Param vb.); rakip değil tedarikçi.
- Aggregator commodity bir hizmet; ölçek büyüdükçe maliyetli olunca direkt entegrasyona geçilir.

### Geçiş Stratejisi

| Faz | Süre | Yaklaşım | Banka kapsamı |
|---|---|---|---|
| Faz 1 (MVP) | 0-3 ay | Tek aggregator | 8 büyük banka (top 80% pazar) |
| Faz 2 | 3-9 ay | Aggregator + top 5'e direkt entegrasyon | Top 5 direkt, gerisi aggregator |
| Faz 3 | 9-18 ay | Direkt entegrasyon olgun | Top 5 direkt (~%70 işlem), gerisi aggregator |

Direkt entegrasyona geçiş tetikleyicisi: yıllık aggregator komisyonu, in-house dev + maintenance maliyetinin 2x'ini aşınca.

### Riskler ve Mitigasyon

| Risk | Mitigasyon |
|---|---|
| Aggregator downtime → bütçe down | SLA sözleşmede ≥%99.5; uzun vadede 2 aggregator (failover) |
| Komisyon ölçek büyüdükçe ağırlaşır | 6 ayda bir maliyet review, direkt entegrasyon planı raflarda hazır |
| Vendor lock-in | Internal canonical data model (aggregator-agnostic); değişimde sadece adapter değişir |
| Veri privacy (3. parti) | AISP zaten BDDK denetimli; sözleşmede data retention, silme, audit log şartları |

### Vendor Seçim Kriterleri (Sıralı)

1. BDDK AISP lisansı (zorunlu)
2. Top 10 bankanın tamamı kapsamda
3. SLA ≥%99.5
4. Webhook desteği (polling değil push)
5. Sandbox + production ayrı ortamlar
6. Komisyon yapısı: per-call vs. per-user vs. flat — ölçek modeline uygun olan
7. Müşteri referansları (finansal hizmet sektöründen)

### MVP Teknik Scope

- 1 aggregator entegrasyonu
- Token yönetimi: 90 gün yenileme akışı (BDDK gereği)
- Webhook: yeni işlem push notification
- Reconciliation: günlük tam senkronizasyon (kayıp event recovery)
- Internal canonical model:
  ```
  Transaction {
    id, amount, currency, date,
    raw_description, merchant_name, mcc,
    bank_account_id, source_aggregator
  }
  ```

---

## 2. Kategorileme

### Seçenekler

| Yaklaşım | Doğruluk | Açıklanabilirlik | Cold start | Maintenance |
|---|---|---|---|---|
| Tam kural bazlı (lookup + regex) | ~%80-85 | Yüksek | Yok | Manuel rule güncellemesi |
| ML-only (text classifier) | %90+ olası | Düşük | Yüksek (training data) | Model retraining pipeline |
| Hibrit: kural önce, ML fallback | %95+ kademeli | Core'da yüksek | Düşük | İki sistem ama sınırlı |

### Karar: Hibrit, kural-öncelikli

### 4 Katmanlı Mimari

```
İşlem geldi
    ↓
[1] MCC kodu var mı? → Visa/MC standart kategori → ATA
    ↓ (yok/belirsiz)
[2] Merchant lookup table (~500 merchant) → Migros = Market → ATA
    ↓ (yok)
[3] Regex/keyword rules → "PETROL OFISI" → Ulaşım → ATA
    ↓ (eşleşme yok)
[4] ML classifier (Faz 2+) → confidence < %70 ise "Diğer"
    ↓
Kullanıcı düzeltebilir → düzeltme = label, gelecek işlemlere uygulanır
```

### Katman Detayları

#### Katman 1 — MCC (Merchant Category Code)
- Visa/Mastercard standardı, banka API'lerinde 4 haneli kod
- Örnek: 5411 → Grocery Stores → Market
- Avantaj: standart, deterministik
- Dezavantaj: MCC eksik veya yanlış olabilir; kapsama ~%40-50

#### Katman 2 — Merchant Lookup Table
- Türkiye'ye özel ~500 popüler merchant
- Örnek girdiler: Migros, A101, BİM, ŞOK, Shell, Opet, Total, Starbucks, Spotify, Netflix, BluTV, Trendyol, Hepsiburada, Yemeksepeti, Getir, Uber, BiTaksi
- Her merchant için: kategori + alt kategori + alias listesi (banka açıklamalarındaki varyasyonlar için)
- Maintenance: ayda 1 review, yeni popüler merchant ekleme

#### Katman 3 — Regex/Keyword Rules
- "POS-XYZ MARKET" → Market
- "ECZ.*" / "DEPO.*ECZA" → Sağlık
- "KIRA" + EFT/havale → Ev & Faturalar
- Tag-based ranking: en spesifik kural önce eşleşir

#### Katman 4 — ML Classifier (Faz 2'de devreye)
- **Ne zaman:** ~6 ay sonra, ~1M+ etiketli işlem birikince
- **Model:** Lightweight text classifier (FastText veya distilled BERT-base TR)
- **Input:** raw_description + merchant_name + amount + day_of_month + day_of_week
- **Output:** kategori + confidence score
- **Eşik:** confidence < %70 → "Diğer" + kullanıcıya sor
- **Active learning:** Kullanıcı düzeltmeleri haftalık retraining loop'una girer

### Doğruluk Hedefleri

| Faz | Katmanlar | Hedef doğruluk | Kullanıcı düzeltme oranı |
|---|---|---|---|
| Faz 1 (MVP) | 1+2+3 | ≥%85 | ≤%15 |
| Faz 2 | + 4 (ML) | ≥%95 | ≤%5 |
| Faz 3 | Active learning olgun | ≥%97 | ≤%3 |

### Açıklanabilirlik (Kullanıcı Tarafı)

Her kategori atamasında "Neden bu kategori?" tıklanabilir:
- Katman 1-3: "Migros → Market kuralı" (deterministik açıklama)
- Katman 4: "Benzer işlemlere göre tahmin" + düşük confidence göstergesi (örn. soluk renk)

### Kullanıcı Düzeltme Akışı

1. İşlem listesinde kategori chip'ine tıkla
2. Yeni kategori seç
3. Sistem sorar: "Bu merchant için **her zaman** mı, **sadece bu işlem** için mi?"
4. "Her zaman" → user-specific override + global learning signal (anonim)

---

## 3. HangiKredi Hesap Entegrasyonu

**Karar:** Bütçe yönetimi ayrı hesap istemez. Mevcut HangiKredi kullanıcı profili = bütçe kullanıcı profili.

### Implikasyonlar

- **Tek SSO, tek profil:** Kullanıcı HK'ya giriş yapmışsa bütçeye de erişebilir.
- **Geçmiş veriler entegre:** Önceki kredi başvuruları, ürün karşılaştırmaları, bekleyen başvurular bütçe öneri motoruna girdi olur.
- **CRM segmentasyonu:** Bütçe kullanan kullanıcı HK ana akışlarında özel segment etiketi alır (daha yüksek LTV beklentisi).
- **KVKK:** Hesap için tek privacy policy. Banka verisi için ayrı opt-in (BDDK rıza akışı zaten ayrı zorunlu).
- **Onboarding:** Kullanıcı HK üyesi değilse → önce HK üyeliği akışı → sonra bütçe.

### Veri Erişim Matrisi

| Veri | Kaynak | Erişim |
|---|---|---|
| Kullanıcı profili (ad, email, telefon) | HK CRM | Bütçe servisi okur |
| Geçmiş başvurular | HK ürün geçmişi | Öneri motoru girdisi |
| Banka işlemleri | Aggregator → Bütçe DB | Sadece kullanıcı + öneri motoru |
| Finansal Sağlık Skoru | Bütçe DB hesaplar | HK ana akışları okuyabilir (öneri için) |

---

## Açık Kalan Teknik Sorular

- [ ] Aggregator vendor seçimi (Fincloud / Param / diğer) — sözleşme öncesi pilot
- [ ] MCC ↔ HK kategori final mapping tablosu (~500 merchant)
- [ ] ML training data: etiketli işlemlerin saklama ve anonimleştirme politikası
- [ ] Multi-currency: TL dışı işlemler için kategorileme ve dashboard davranışı (Faz 2+)
- [ ] HK profilindeki KKB skor verisi öneri motoruna girer mi? (KVKK + iç compliance review)
