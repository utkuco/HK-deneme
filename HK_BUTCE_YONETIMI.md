# HangiKredi — Bütçe Yönetimi Özelliği

> **Durum:** Taslak  
> **Tarih:** 2026-06-08  
> **Kapsam:** Standalone özellik + Açık Bankacılık (OBS/HES) entegrasyonu

---

## Neden HangiKredi?

HangiKredi'nin temel değeri: kullanıcıya en uygun finansal ürünü bulmak.  
Bütçe yönetimi bu değeri bir üst seviyeye taşır:

> *"Sana en iyi krediyi bulmakla kalmıyoruz, finansal hayatını yönetmene de yardım ediyoruz."*

**Kritik fark:** Rakip bütçe uygulamalarının aksine HangiKredi, harcama verisini **ürün önerisiyle birleştirebilir.**  
Örnek: "Kredi kartı faizine bu ay ₺2.400 ödedin → Sana daha uygun bir kart var."

---

## Kullanıcı Değeri

| Kullanıcı Sorunu | HangiKredi Çözümü |
|------------------|-------------------|
| Param nereye gidiyor bilmiyorum | Banka bağlantısı → otomatik kategori |
| Ay sonunu getiremiyorum | Harcama limitleri + uyarılar |
| Kredi taksitlerimi takip edemiyorum | Taahhüt takibi + kalan ödeme |
| Daha iyi finansal ürün var mı bilmiyorum | Harcama analizi → kişiselleştirilmiş öneri |

---

## Açık Bankacılık (OBS/HES) Entegrasyonu

### Türkiye'deki Çerçeve
- **BDDK Açık Bankacılık API'leri** — tüm bankalar zorunlu
- **HES (Hesap Erişim Servisi):** hesap bakiyesi + işlem geçmişi okuma
- **OBS (Ödeme Başlatma Servisi):** ödeme tetikleme (sonraki fazda)
- Kullanıcı onayı: her banka bağlantısı için tek seferlik rıza (90 gün yenilenebilir)

### Bağlanabilecek Hesaplar
- Vadesiz mevduat hesapları
- Kredi kartları (harcama + ekstre)
- Krediler (taksit planı)
- Katılım bankası hesapları

### Veri Akışı
```
Kullanıcı → Banka seç → Banka rıza ekranı (redirect) → Token al
→ HangiKredi işlemleri çeker → Otomatik kategorileme → Dashboard
```

### Güvenlik
- Token'lar şifreli saklanır, HangiKredi banka şifresi görmez
- Kullanıcı istediği zaman bağlantıyı iptal edebilir
- Sadece okuma yetkisi (HES), para transferi yok

---

## Özellikler

### Faz 1 — MVP

#### Hesap Bağlama
- Desteklenen bankalar listesi (Garanti, İş, Akbank, YKB, Ziraat, vb.)
- Banka seçimi → BDDK onaylı rıza akışı
- Çoklu hesap bağlama (farklı bankalar)
- Bağlantı durumu ve son güncelleme zamanı

#### Otomatik İşlem Çekme
- Bağlı hesaplardan son 3-6 ay işlem geçmişi
- Yeni işlem gelince bildirim
- Manuel işlem ekleme (nakit harcamalar için)

#### Otomatik Kategorileme
- AI/kural bazlı: "Migros" → Market, "Shell" → Ulaşım
- Kullanıcı kategori düzeltebilir → model öğrenir
- Varsayılan kategoriler:
  - 🛒 Market & Alışveriş
  - 🍔 Yeme & İçme
  - 🚗 Ulaşım & Akaryakıt
  - 🏠 Ev & Faturalar
  - 💊 Sağlık
  - 🎬 Eğlence & Abonelikler
  - 📚 Eğitim
  - 💳 Kredi & Taksit
  - 💰 Diğer

#### Dashboard
- Net bakiye (toplam varlık - toplam borç)
- Bu ay: gelir / gider / net
- Kategori bazlı harcama dağılımı
- Geçen aya göre değişim (% fark)
- Yaklaşan ödemeler (kredi taksitleri, faturalar)

#### Bütçe Limitleri
- Kategori bazlı aylık limit tanımlama
- Limite %80 ulaşınca push bildirim
- Limit aşımında uyarı

### Faz 2 — Büyüme

#### Taahhüt Takibi
- Aktif kredi taksitleri (banka bağlantısından otomatik)
- Kalan ödeme sayısı ve toplam tutar
- "Bu krediyi erken kapatsan ne kadar tasarruf edersin?"

#### Tasarruf Hedefleri
- Hedef adı + tutar + tarih
- "Bu hedefe ulaşmak için ayda ₺X ayırman lazım"
- Otomatik ilerleme takibi

#### Akıllı Raporlar
- Aylık harcama raporu (email/PDF)
- Kategori trendi (son 6 ay)
- En yüksek harcama günleri

### Faz 3 — HangiKredi Entegrasyonu (Kritik Farklılaşma)

#### Kişiselleştirilmiş Ürün Önerileri
- "Kredi kartı faizine bu ay ₺2.400 ödedin → Şu kart limit sunuyor"
- "3 farklı bankada hesabın var → Bunları birleştirsen ₺X faiz kazanırsın"
- "Kredi taksitlerin gelirine oranı %42 → Yeniden yapılandırma seçenekleri"
- "Market harcaman yüksek → Bu kartın cashback'i sana uygun"

#### Finansal Sağlık Skoru
- 0-100 arası skor (borç/gelir oranı, tasarruf oranı, limit kullanımı)
- Skoru iyileştirmek için öneriler
- Kredi başvurusunda önceden "başvursan onaylanır mı?" tahmini

---

## Ekranlar

| Ekran | İçerik |
|-------|--------|
| Ana sayfa / Dashboard | Bakiye, bu ay özeti, son işlemler, hızlı uyarılar |
| İşlemler | Tarih sıralı liste, filtre/arama, kategori düzenleme |
| Bütçe | Kategori limitler, ilerleme çubukları |
| Hesaplar | Bağlı bankalar, bakiyeler, bağlantı yönetimi |
| Hedefler | Tasarruf hedefleri |
| Öneriler | HangiKredi ürün önerileri (kart, kredi, mevduat) |
| Ayarlar | Bildirimler, kategoriler, hesap yönetimi |

---

## Kullanıcı Akışı — İlk Kurulum

```
Uygulama aç → "Bütçeni Yönet" banner
→ Banka bağla ekranı
→ Banka seç (liste)
→ Banka uygulamasına/web'e yönlendir (rıza onayı)
→ Geri dön → "Hesaplar bağlandı"
→ İşlemler yükleniyor (son 3 ay)
→ Dashboard hazır
→ "Kategorileri düzenlemek ister misin?" → opsiyonel tur
```

---

## Teknik Notlar

### Mimari
- Backend: işlem çekme + kategorileme servisi
- Webhook / polling: yeni işlem bildirimleri için
- Veri saklama: kullanıcı bazlı şifreli işlem DB
- Kategorileme: kural motoru + ML (merchant name → kategori)

### BDDK Uyumluluğu
- Rıza yönetimi zorunlu (kullanıcı dilediğinde iptal)
- Veri saklama politikası açıklanmalı
- 90 günlük token yenileme akışı

### Öncelikli Bankalar (Pazar payına göre)
1. Garanti BBVA
2. İş Bankası
3. Akbank
4. Yapı Kredi
5. Ziraat Bankası
6. Halkbank / Vakıfbank
7. QNB Finansbank
8. Denizbank

---

## Açık Sorular

- [ ] Kategorileme için ML mi, kural motoru mu, ikisi birden mi?
- [ ] Banka bağlantısı için 3. parti aggregator (Fincloud, Param vb.) mı yoksa direkt BDDK API mi?
- [ ] Veri ne kadar süre saklanacak?
- [ ] Önce iOS mu Android mi Web mi?
- [ ] Bütçe özelliği premium mi (ücretli) yoksa ücretsiz mi?
- [ ] Mevcut HangiKredi kullanıcı hesabıyla mı entegre?

---

## Başarı Metrikleri

| Metrik | Hedef |
|--------|-------|
| Banka bağlama oranı (onboarding'de) | >%40 |
| 30 gün retention | >%50 |
| Haftada en az 1 açma | >%35 |
| Öneri tıklama oranı | >%15 |
| Bağlanan hesap başına ürün başvurusu | Mevcut ortalamadan +%30 |
