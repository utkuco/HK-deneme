# CONTEXT.md — Ortak Proje Bağlamı

> Bu dosya her iki taraf tarafından güncellenir.

---

## Son Güncelleme
- **Tarih:** 2026-06-08
- **Güncelleyen:** Seran (Claude aracılığıyla)

---

## Proje Özeti

**HangiKredi Bütçe Yönetimi** — Açık bankacılık (OBS/HES) ile banka hesaplarını bağlayarak otomatik harcama takibi, kategori bazlı bütçe ve HangiKredi ürün önerileriyle entegre kişisel finans aracı.

Dökümanlar:
- [`HK_BUTCE_YONETIMI.md`](./HK_BUTCE_YONETIMI.md) — Ana ürün dökümanı (detaylı)
- [`BUTCE_YONETIMI.md`](./BUTCE_YONETIMI.md) — Genel bütçe araç konsepti

---

## Alınan Kararlar

**Ürün (Utku, 2026-06-08):**
- ✅ Standalone özellik olarak konumlanacak
- ✅ Veri kaynağı: Açık Bankacılık (OBS/HES), manuel giriş değil
- ✅ 3 fazlı yol haritası: MVP → Büyüme → HK Entegrasyonu

**Workflow (Seran, 2026-06-08):**
- [x] Seran tarafı Claude oturumlarında repo'ya otomatik commit/push açıldı. `gh` CLI kuruldu, auth aktif. Her önemli karar anında Claude CONTEXT.md güncellemesini onay için sunuyor, onay sonrası push ediyor. Manuel kopyala-yapıştır akışı kaldırıldı. Detay: [`SERAN_GUIDE.md`](./SERAN_GUIDE.md)

---

## Açık Sorular / Bekleyen Maddeler
- [ ] Kategorileme: ML mi, kural motoru mu?
- [ ] Banka bağlantısı: 3. parti aggregator mı, direkt BDDK API mi?
- [ ] Platform önceliği: iOS / Android / Web?
- [ ] Premium mi, ücretsiz mi?
- [ ] Mevcut HK kullanıcı hesabıyla entegre mi?

---

## Son Oturum Özeti (Utku)
**2026-06-08:** HangiKredi bütçe yönetimi özelliği tasarlandı. Açık bankacılık entegrasyonu, 3 fazlı roadmap, ekranlar, başarı metrikleri ve teknik notlar dökümanlandı. Kritik farklılaşma: harcama verisi + ürün öneri motoru kombinasyonu.

---

## Son Oturum Özeti (Seran)
**2026-06-08:** İlk oturum. Repo workflow'u otomasyona alındı:
- `gh` CLI tarball ile `~/.local/gh/` altına kuruldu (Homebrew sahipliği bozuk olduğu için brew kullanılamadı).
- `gh auth login` ile auth aktif edildi; `seranpoyraz` olarak push yetkisi doğrulandı.
- Claude memory'sine "her konuşmada CONTEXT.md'yi oku, önemli karar anında onayla-commit-push" kuralı yazıldı (`feedback-hk-context-update`).
- `SERAN_GUIDE.md` yeni otomatik akışı yansıtacak şekilde güncellendi.
- Bütçe yönetimi açık sorularına Seran tarafından henüz cevap verilmedi — bir sonraki oturumda ele alınacak.
