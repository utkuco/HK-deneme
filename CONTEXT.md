# CONTEXT.md — Ortak Proje Bağlamı

---

## Son Güncelleme
- **Tarih:** 2026-06-08
- **Güncelleyen:** Seran (Claude aracılığıyla)

---

## Proje Özeti

**HangiKredi Bütçe Yönetimi** — Açık bankacılık (OBS/HES) ile otomatik harcama takibi + HangiKredi ürün öneri motoruyla entegre kişisel finans aracı.

### Dökümanlar
| Dosya | İçerik |
|-------|--------|
| [`HK_BUTCE_YONETIMI.md`](./HK_BUTCE_YONETIMI.md) | Genel ürün dökümanı, 3 fazlı roadmap |
| [`HK_ENTEGRASYON_DETAY.md`](./HK_ENTEGRASYON_DETAY.md) | HK entegrasyonu: finansal profil, öneri motoru, skor |
| [`HK_VERI_KATMANI.md`](./HK_VERI_KATMANI.md) | Banka bağlantısı + kategorileme + HK hesap entegrasyonu |
| [`BUTCE_YONETIMI.md`](./BUTCE_YONETIMI.md) | Genel bütçe araç konsepti |

---

## Alınan Kararlar
- ✅ Standalone özellik
- ✅ Veri kaynağı: Açık Bankacılık (OBS/HES)
- ✅ 3 fazlı roadmap (MVP → Büyüme → HK Entegrasyonu)
- ✅ Öneri motoru: trigger → insight → öneri → aksiyon zinciri
- ✅ Finansal Sağlık Skoru (0-100) entegre edilecek
- ✅ Öneri tonu: reklam değil, danışman
- ✅ **Banka bağlantısı (Seran, 2026-06-08):** 3. parti aggregator (AISP) ile başla, 6-12 ay sonra top 5 bankaya direkt geç. Detay: [`HK_VERI_KATMANI.md`](./HK_VERI_KATMANI.md)
- ✅ **Kategorileme (Seran, 2026-06-08):** Hibrit, kural-öncelikli (MCC + lookup + regex). MVP hedefi %85 doğruluk. Faz 2'de ML fallback (%95+). Detay: [`HK_VERI_KATMANI.md`](./HK_VERI_KATMANI.md)
- ✅ **HK hesap entegrasyonu (Seran, 2026-06-08):** Mevcut HangiKredi kullanıcı hesabı = bütçe hesabı. Ayrı hesap yok, tek SSO. Detay: [`HK_VERI_KATMANI.md`](./HK_VERI_KATMANI.md)

---

## Açık Sorular
- [ ] Platform önceliği: iOS / Android / Web?
- [ ] Premium mi, ücretsiz mi?
- [ ] Öneri motoru kural bazlı mı başlasın, ML ne zaman? (kategorileme için yanıtlandı; öneri motoru için açık)
- [ ] Banka bağlantısı olmayan kullanıcılara öneri nasıl?
- [ ] Sigorta önerileri hangi fazda?
- [ ] Aggregator vendor seçimi (Fincloud / Param / diğer)
- [ ] Veri saklama ve anonimleştirme politikası (ML training için)

---

## Son Oturum Özeti (Utku)
**2026-06-08:** HK entegrasyonu detaylandırıldı. Finansal profil katmanları (gelir/borç/harcama/tasarruf), öneri motoru mantığı (trigger→insight→öneri), kredi kartı/kredi/mevduat/sigorta öneri kuralları, Finansal Sağlık Skoru (0-100 bileşenleri), öneri sunuş deneyimi (danışman tonu) ve teknik mimari dökümanlandı.

---

## Son Oturum Özeti (Seran)
**2026-06-08:** İki oturum tamamlandı.

**1. oturum — Workflow:** Repo otomasyonu kuruldu. `gh` CLI tarball ile kuruldu, auth aktif (`seranpoyraz`). Claude memory'sine "her konuşmada CONTEXT.md'yi pull et + önemli karar anında onay-commit-push" kuralı yazıldı. `SERAN_GUIDE.md` yeni akışa göre güncellendi.

**2. oturum — Veri katmanı kararları:** Üç teknik karar alındı, [`HK_VERI_KATMANI.md`](./HK_VERI_KATMANI.md) dökümanında detaylandırıldı:
- **Banka bağlantısı:** 3. parti aggregator (AISP) ile başla, top 5 bankaya 6-12 ay sonra direkt geçiş.
- **Kategorileme:** 4 katmanlı hibrit — MCC → merchant lookup (~500) → regex → ML fallback (Faz 2). MVP'de %85, Faz 2'de %95 doğruluk hedefi.
- **HK hesap entegrasyonu:** Tek SSO, mevcut HK profili = bütçe profili. Geçmiş başvurular öneri motoruna girdi.

`HK_BUTCE_YONETIMI.md`'deki ilgili açık sorular yanıtlandı olarak işaretlendi.
