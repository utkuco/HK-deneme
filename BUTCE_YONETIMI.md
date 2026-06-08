# Bütçe Yönetimi Aracı — Ürün Dökümanı

> **Durum:** Taslak  
> **Başlangıç:** 2026-06-08  
> **Katkıda bulunanlar:** Utku, Seran

---

## Vizyon

Kişisel ve ortak bütçeleri kolayca takip etmeyi, harcama alışkanlıklarını anlamayı ve finansal hedeflere ulaşmayı sağlayan sade bir araç.

---

## Temel Özellikler

### 1. Gelir & Gider Girişi
- Manuel işlem ekleme (tarih, tutar, kategori, not)
- Tekrarlayan işlemler (aylık kira, abonelikler vb.)
- Çoklu hesap desteği (nakit, banka hesabı, kredi kartı)

### 2. Kategoriler
- Varsayılan kategoriler: Yiyecek, Ulaşım, Faturalar, Eğlence, Sağlık, Giyim, Eğitim, Diğer
- Özel kategori oluşturma
- Her kategoriye renk/ikon atama

### 3. Bütçe Hedefleri
- Aylık harcama limiti belirleme (genel veya kategori bazlı)
- Limite yaklaşınca uyarı
- Limit aşımında bildirim

### 4. Dashboard
- Bu ay: toplam gelir / toplam gider / net bakiye
- Kategori bazlı harcama dağılımı (pasta grafik)
- Günlük harcama trendi (çizgi grafik)
- Kalan bütçe göstergesi

### 5. Tasarruf Hedefleri
- Hedef adı, tutar, bitiş tarihi
- İlerleme çubuğu
- Otomatik "bu hedefe ne kadar ayırmalıyım?" hesabı

### 6. Raporlar
- Aylık/yıllık özet
- Kategori karşılaştırması (bu ay vs geçen ay)
- Export: CSV, PDF

### 7. Borç / Alacak Takibi
- Kime borçlusun / kimden alacaklısın
- Ödeme hatırlatıcısı
- Kısmi ödeme kaydı

---

## Kullanıcı Akışları

### Yeni işlem ekle
```
Ana ekran → "+" butonu → Tutar gir → Kategori seç → Tarih seç → Kaydet
```

### Aylık bütçe kur
```
Ayarlar → Bütçe Hedefleri → Kategori seç → Limit gir → Kaydet
```

### Tasarruf hedefi oluştur
```
Hedefler → "Yeni Hedef" → Ad + Tutar + Tarih → Kaydet
```

---

## Ekranlar (İlk Versiyon)

| Ekran | Açıklama |
|-------|----------|
| Dashboard | Ana özet, güncel bakiye, hızlı ekleme |
| İşlemler | Tüm gelir/gider listesi, filtreleme |
| Bütçe | Kategori limitler ve ilerleme |
| Hedefler | Tasarruf hedefleri |
| Raporlar | Grafikler ve export |
| Ayarlar | Hesaplar, kategoriler, bildirimler |

---

## Açık Sorular

- [ ] Platform ne olacak? (Web / Mobil / Her ikisi)
- [ ] Tek kullanıcı mı, çok kullanıcı/aile bütçesi mi?
- [ ] Banka entegrasyonu olacak mı? (otomatik işlem çekme)
- [ ] Para birimi desteği (sadece TL mi?)
- [ ] Backend gerekli mi, local-first mi?
- [ ] Hangi teknoloji stack kullanılacak?

---

## Notlar

<!-- Toplantı notları, fikirler buraya -->
