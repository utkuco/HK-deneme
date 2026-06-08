# HangiKredi Entegrasyonu — Detaylı Döküman

> **Bağlı döküman:** [`HK_BUTCE_YONETIMI.md`](./HK_BUTCE_YONETIMI.md)  
> **Tarih:** 2026-06-08

---

## Temel Fikir

Bütçe verisi tek başına değerli. Ama HangiKredi için asıl güç şuradan geliyor:

```
Kullanıcının harcama davranışı
        ↓
Finansal profil çıkarma
        ↓
Kişiselleştirilmiş ürün eşleştirme
        ↓
Doğru anda, doğru öneri
        ↓
Yüksek dönüşümlü başvuru
```

Rakip bütçe uygulamaları (Tosla, Bütçem, vb.) harcama gösterir, bitirir.  
HangiKredi harcamayı gösterir **ve bir sonraki adım önerir.**

---

## Finansal Profil Katmanları

Banka verilerinden çıkarılabilecek profil bileşenleri:

### 1. Gelir Profili
| Sinyal | Nasıl tespit edilir |
|--------|---------------------|
| Aylık net gelir | Düzenli büyük gelen EFT/havale (maaş) |
| Gelir düzenliliği | Sabit maaşlı mı, serbest çalışan mı? |
| Ek gelir kaynakları | Kira geliri, freelance ödemeleri |
| Gelir trendi | Son 6 ayda artış/azalış |

### 2. Borç Profili
| Sinyal | Nasıl tespit edilir |
|--------|---------------------|
| Aktif kredi taksitleri | Düzenli banka EFT çıkışları |
| Kredi kartı borcu | Ekstre ödemeleri (tam mı, asgari mi?) |
| Borç/gelir oranı (DTI) | Taksitler / aylık gelir |
| Kredi kartı kullanım oranı | Harcama / limit |

### 3. Harcama Profili
| Sinyal | Değer |
|--------|-------|
| Kategori dağılımı | Hangi kategoride ne kadar harcıyor |
| Yüksek faizli harcama | Market, yeme-içme (düşük değer) vs. yatırım |
| Abonelik harcamaları | Netflix, Spotify, gym vb. |
| Yakıt / ulaşım harcaması | Araç sahibi mi? |
| Kira ödemesi | Kiracı mı? |

### 4. Tasarruf Profili
| Sinyal | Nasıl tespit edilir |
|--------|---------------------|
| Aylık tasarruf oranı | (Gelir - Gider) / Gelir |
| Atıl nakit | Vadesiz hesapta uzun süre bekleyen para |
| Mevduat / yatırım hareketi | Fon/mevduat transferleri |

---

## Öneri Motorunun Çalışma Mantığı

Her kullanıcı için **trigger → insight → öneri → aksiyon** zinciri:

### Kredi Kartı Önerileri

| Trigger (Harcama Sinyali) | Insight | Öneri |
|--------------------------|---------|-------|
| Market harcaması yüksek (>₺5K/ay) | Cashback fırsatı var | "Migros/CarrefourSA anlaşmalı kart" |
| Akaryakıt harcaması yüksek (>₺3K/ay) | Yakıt indirimi kaçıyor | "Shell/Opet cashback kartı" |
| Yeme-içme harcaması yüksek | Restoran puanı kaçıyor | "Miles&Smiles / yemek cashback kartı" |
| Kredi kartı faizi ödeniyor | Gereksiz faiz gidiyor | "0 faiz taksit limiti yüksek kart" |
| Sadece asgari ödeme yapılıyor | Borç büyüyor | "Borç transferi / daha düşük faizli kart" |
| Çok sayıda kart var | Karmaşıklık + limit dağınıklığı | "Bu kartları konsolide et" |

### Kredi Önerileri

| Trigger | Insight | Öneri |
|---------|---------|-------|
| DTI oranı >%40 | Borç yükü ağır | "Borç yapılandırma / refinansman" |
| Yüksek faizli birden fazla kredi | Faiz optimizasyonu mümkün | "Kredi birleştirme (konsolidasyon)" |
| Düzenli tasarruf + düşük borç | Kredi için iyi profil | "Konut kredisi ön onay skoru" |
| Büyük tek seferlik harcama | Nakit ihtiyacı | "İhtiyaç kredisi teklifi" |
| Araç bakım/yakıt artışı | Araç değiştirmeyi düşünüyor olabilir | "Taşıt kredisi teklifleri" |

### Mevduat / Yatırım Önerileri

| Trigger | Insight | Öneri |
|---------|---------|-------|
| Vadesiz hesapta >₺50K atıl para | Para çalışmıyor | "TL/döviz vadeli mevduat teklifleri" |
| Düzenli aylık tasarruf (>₺5K) | Birikim alışkanlığı var | "Otomatik yatırım planı / fon" |
| Gelir artışı (son 3 ay) | Ek tasarruf kapasitesi | "Yatırım başlama fırsatı" |
| Kira geliri var | Gayrimenkul yatırımcısı | "Yüksek getirili mevduat / GYO" |

### Sigorta Önerileri

| Trigger | Insight | Öneri |
|---------|---------|-------|
| Araç yakıt/bakım harcaması | Araç sahibi | "Kasko / trafik sigortası karşılaştır" |
| Kira ödemesi var | Kiracı | "Konut / eşya sigortası" |
| Sağlık harcaması yüksek | Sağlık giderleri ağır | "Özel sağlık sigortası teklifi" |
| Maaş düştü (işsizlik?) | Gelir riski | "Hayat / işsizlik sigortası" |

---

## Finansal Sağlık Skoru

Kullanıcıya 0-100 arası tek bir skor → hem motivasyon, hem ürün eşleştirme girdisi.

### Skor Bileşenleri

| Bileşen | Ağırlık | İdeal |
|---------|---------|-------|
| Borç/Gelir Oranı (DTI) | %35 | < %30 |
| Tasarruf Oranı | %25 | > %20 |
| Kredi Kartı Kullanım Oranı | %20 | < %30 |
| Acil Fon (kaç aylık gider) | %15 | > 3 ay |
| Ödeme Düzenliliği | %5 | 0 gecikme |

### Skor → Öneri Mapping

| Skor | Durum | Öncelikli Öneri |
|------|-------|-----------------|
| 80-100 | Mükemmel | Yatırım ürünleri, premium kart |
| 60-79 | İyi | Daha iyi kart, mevduat optimizasyonu |
| 40-59 | Orta | Borç yapılandırma, bütçe limitleri |
| 0-39 | Dikkat | Acil kredi konsolidasyonu, finansal danışmanlık |

---

## Öneri Sunuş Deneyimi

### Nerede Gösterilecek?

1. **Dashboard'da "Senin için"** kartı — 1-2 öneri, her hafta güncellenir
2. **İşlem detayında bağlamsal** — "Bu harcama için daha iyi bir kart var →"
3. **Aylık özet bildirimi** — "Ocak ayında X faiz ödedin. Şu kartla Y tasarruf ederdin."
4. **Finansal Sağlık Skoru ekranında** — Skoru artırmak için öneriler listesi

### Nasıl Sunulacak? (Ton)

❌ Reklam gibi değil:
> "Garanti Bonus Kart'a hemen başvur!"

✅ Danışman gibi:
> "Bu ay market alışverişine ₺4.200 harcadın. Bonus Kart ile ₺210 geri alabilirdin. Karşılaştır →"

### Öneri Kartı Anatomisi
```
┌─────────────────────────────────────────┐
│ 💡 Bu ay ₺340 faiz ödedin               │
│                                         │
│ Kredi kartı borcunu daha düşük faizli   │
│ bir karta taşısan yıllık ₺1.800         │
│ tasarruf edebilirsin.                   │
│                                         │
│ [Kartları Karşılaştır →]                │
│                                         │
│ ⚡ 3 kart uygun · Anlık onay mümkün     │
└─────────────────────────────────────────┘
```

---

## Teknik Mimari

```
┌──────────────────────────────────────────────────────┐
│                   Bütçe Servisi                       │
│  İşlemler → Kategori → Profil Hesaplama              │
└─────────────────────┬────────────────────────────────┘
                      │ Finansal Profil
                      ▼
┌──────────────────────────────────────────────────────┐
│              Öneri Motoru (Rules Engine)              │
│  Trigger kuralları → Ürün eşleştirme → Sıralama      │
└─────────────────────┬────────────────────────────────┘
                      │ Öneri listesi
                      ▼
┌──────────────────────────────────────────────────────┐
│            HangiKredi Ürün Kataloğu API               │
│  Kredi / Kart / Mevduat / Sigorta güncel teklifleri  │
└─────────────────────┬────────────────────────────────┘
                      │ Kişiselleştirilmiş teklifler
                      ▼
┌──────────────────────────────────────────────────────┐
│               Kullanıcı Arayüzü                       │
│  Dashboard · İşlem detayı · Bildirim · Skor ekranı   │
└──────────────────────────────────────────────────────┘
```

---

## Öneri Motoru — Kural Örnekleri (Pseudocode)

```python
# Kredi kartı faiz ödüyor mu?
if user.credit_card_interest_paid_monthly > 0:
    savings = calculate_potential_savings(user.card_balance, better_rate)
    if savings > 500:  # TL/yıl eşiği
        recommend("balance_transfer_card", priority="HIGH",
                  message=f"Bu ay ₺{interest} faiz ödedin. Taşısan yılda ₺{savings} tasarruf.")

# Market harcaması yüksek mi?
if user.category_spend["market"] > 3000:  # TL/ay
    best_cashback_card = find_best_cashback_card("market", user.profile)
    recommend("cashback_card", card=best_cashback_card,
              message=f"Market alışverişinde ayda ₺{potential_cashback} geri alabilirsin.")

# Atıl nakit var mı?
idle_cash = user.avg_checking_balance - user.monthly_expenses * 2
if idle_cash > 20000:
    best_deposit = find_best_deposit_rate(idle_cash, user.preferred_currency)
    recommend("deposit", amount=idle_cash,
              message=f"₺{idle_cash:,} paranı çalıştırsan ayda ₺{monthly_interest} faiz kazanırsın.")
```

---

## Başarı Metrikleri (Entegrasyon Özelinde)

| Metrik | Hedef |
|--------|-------|
| Öneri gösterim → tıklama oranı (CTR) | >%20 |
| Tıklama → başvuru oranı | >%35 |
| Bütçe kullanıcısı başvuru oranı vs. normal kullanıcı | 2x |
| Öneri başına ortalama gelir (komisyon) | Mevcut ortalamadan +%40 |
| Yanlış öneri şikayeti oranı | <%2 |

---

## Açık Sorular

- [ ] Öneri motoru kural bazlı mı başlasın, ML ne zaman devreye girsin?
- [ ] Kaç öneri aynı anda gösterilecek? (Öneri yorgunluğu riski)
- [ ] Kullanıcı "bu öneriyi gösterme" diyebilecek mi?
- [ ] Banka bağlantısı olmayan kullanıcılara öneri nasıl çalışacak? (sadece anket bazlı?)
- [ ] Sigorta önerileri hangi fazda?
