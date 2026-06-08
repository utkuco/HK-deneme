# CONTEXT.md — Ortak Proje Bağlamı

---

## Son Güncelleme
- **Tarih:** 2026-06-08
- **Güncelleyen:** Utku

---

## Proje Özeti

**HangiKredi Bütçe Yönetimi** — Açık bankacılık (OBS/HES) ile otomatik harcama takibi + HangiKredi ürün öneri motoruyla entegre kişisel finans aracı.

### Dökümanlar
| Dosya | İçerik |
|-------|--------|
| [`HK_BUTCE_YONETIMI.md`](./HK_BUTCE_YONETIMI.md) | Genel ürün dökümanı, 3 fazlı roadmap |
| [`HK_ENTEGRASYON_DETAY.md`](./HK_ENTEGRASYON_DETAY.md) | HK entegrasyonu: finansal profil, öneri motoru, skor |
| [`BUTCE_YONETIMI.md`](./BUTCE_YONETIMI.md) | Genel bütçe araç konsepti |

---

## Alınan Kararlar
- ✅ Standalone özellik
- ✅ Veri kaynağı: Açık Bankacılık (OBS/HES)
- ✅ 3 fazlı roadmap (MVP → Büyüme → HK Entegrasyonu)
- ✅ Öneri motoru: trigger → insight → öneri → aksiyon zinciri
- ✅ Finansal Sağlık Skoru (0-100) entegre edilecek
- ✅ Öneri tonu: reklam değil, danışman

---

## Açık Sorular
- [ ] Banka bağlantısı: 3. parti aggregator mı, direkt BDDK API mi?
- [ ] Platform önceliği: iOS / Android / Web?
- [ ] Premium mi, ücretsiz mi?
- [ ] Öneri motoru: kural bazlı mı başlasın, ML ne zaman?
- [ ] Banka bağlantısı olmayan kullanıcılara öneri nasıl?
- [ ] Sigorta önerileri hangi fazda?

---

## Son Oturum Özeti (Utku)
**2026-06-08:** HK entegrasyonu detaylandırıldı. Finansal profil katmanları (gelir/borç/harcama/tasarruf), öneri motoru mantığı (trigger→insight→öneri), kredi kartı/kredi/mevduat/sigorta öneri kuralları, Finansal Sağlık Skoru (0-100 bileşenleri), öneri sunuş deneyimi (danışman tonu) ve teknik mimari dökümanlandı.

---

## Son Oturum Özeti (Seran)
<!-- Seran güncelleyecek -->
