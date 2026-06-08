# Seran için Claude Kullanım Rehberi

Bu rehber, Utku ile ortak proje üzerinde çalışırken Claude'u nasıl kullanacağını açıklar.

---

## Temel Mantık

Sen ve Utku ayrı ayrı Claude ile çalışıyorsunuz. Aranızdaki ortak hafıza bu GitHub reposundaki `CONTEXT.md` dosyasıdır.

```
Sen  ←→  Claude  ←→  CONTEXT.md  ←→  Claude  ←→  Utku
```

---

## Otomatik Davranış (Seran tarafı)

Seran'ın Claude oturumlarında şu kural memory'de kayıtlı:

> Her konuşmada `https://github.com/utkuco/HK-deneme` reposundaki `CONTEXT.md`
> dosyasını oku. Önemli karar / açık soru / oturum özeti çıktığında ilgili
> bölümü güncelle, Seran'a onay göster, onay sonrası `gh` ile commit + push et.

**Seran'ın sorumluluğu:** Sadece onay vermek. Commit/push'u Claude yapıyor.

`gh` CLI `~/.local/gh/` altına tarball ile kuruldu (Homebrew sahipliği bozuk olduğu için brew kullanılmadı). Auth aktif (`gh auth status` ile doğrulanabilir).

---

## Pratikte Nasıl Çalışıyor

### Oturum başında
Hiçbir şey yapmana gerek yok. Claude memory'sinden kuralı görüyor ve `CONTEXT.md`'yi kendisi okuyor.

### Oturum sırasında / önemli karar anında
Claude şöyle bir şey diyecek:

> CONTEXT.md'nin "Alınan Kararlar" bölümüne şunu ekliyorum:
> ```
> - [x] X yerine Y kullanılacak çünkü Z. (2026-06-08, Seran)
> ```
> Onaylıyor musun?

"Evet" / "tamam" deyince Claude `git commit` + `git push` yapıyor. Mesaj formatı:
```
Seran (Claude): <kısa açıklama>
```

### Oturum sonunda
Claude'a "bu oturumu özetle" dediğinde "Son Oturum Özeti (Seran)" bölümünü hazırlıyor ve aynı onay-commit akışıyla push ediyor.

### Manuel müdahale
İstediğin zaman web UI'dan da edit edebilirsin — Claude her oturum başında `git pull` yapıyor (veya raw fetch ediyor), çakışma olmaz.

---

## Sık Kullanılacak Claude Komutları

| Ne yapmak istiyorsun | Claude'a ne söyleyeceksin |
|----------------------|--------------------------|
| Oturum özetini çıkar | "Bu oturumu CONTEXT.md'ye yaz" |
| Karar al | "X konusunda karar verelim, seçenekleri listele" |
| Bağlamı kontrol et | "Şu ana kadar ne kararlaştırdık?" |
| Utku'ya mesaj bırak | "CONTEXT.md'ye Utku için bir not ekle: ..." |
| Repo'yu yenile | "CONTEXT.md'yi tekrar oku, Utku güncellemiş olabilir" |

---

## Önemli Notlar

- Claude `CONTEXT.md`'yi her oturum başında otomatik okur; sen ayrıca yapıştırmana gerek yok.
- Her commit öncesi Claude sana metni gösterir — sessizce push etmez, onay kapısı her zaman var.
- `BUTCE_YONETIMI.md` gibi ürün dökümanları için de aynı kural geçerli: Claude düzenler, sen onaylarsın, push olur.
- Çakışma olursa (ikisi aynı anda düzenlerse) elle birleştirin.

---

## Repo Linki

👉 https://github.com/utkuco/HK-deneme
