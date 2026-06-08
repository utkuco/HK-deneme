# Seran için Claude Kullanım Rehberi

Bu rehber, Utku ile ortak proje üzerinde çalışırken Claude'u nasıl kullanacağını açıklar.

---

## Temel Mantık

Sen ve Utku ayrı ayrı Claude ile çalışıyorsunuz. Aranızdaki ortak hafıza bu GitHub reposundaki `CONTEXT.md` dosyasıdır.

```
Sen  ←→  Claude  ←→  CONTEXT.md  ←→  Claude  ←→  Utku
```

---

## Her Oturuma Başlarken

1. [`CONTEXT.md`](./CONTEXT.md) dosyasını aç, içeriğini kopyala
2. Claude sohbetini başlat ve şunu söyle:

```
Aşağıdaki proje bağlamını oku, sonra devam edeceğiz:

[CONTEXT.md içeriğini buraya yapıştır]
```

3. Claude artık projenin nerede olduğunu biliyor, çalışmaya başlayabilirsiniz.

---

## Oturum Sonunda (Arada da Yapılabilir)

Önemli bir karar aldığında veya oturum uzadığında Claude'a şunu söyle:

```
Bu oturumu özetle. CONTEXT.md'deki "Son Oturum Özeti (Seran)" bölümüne
yazılacak bir güncelleme hazırla. Ayrıca varsa yeni kararları ve açık soruları da listele.
```

Claude bir metin üretecek. Bunu `CONTEXT.md` dosyasına yapıştır ve GitHub'a push et:

1. GitHub'da `CONTEXT.md` dosyasını aç
2. Sağ üstteki kalem ikonuna tıkla (Edit)
3. İlgili bölümleri güncelle
4. "Commit changes" butonuna bas → "Seran: oturum özeti güncellendi" gibi bir mesaj yaz

---

## Sık Kullanılacak Claude Komutları

| Ne yapmak istiyorsun | Claude'a ne söyleyeceksin |
|----------------------|--------------------------|
| Oturum özetini çıkar | "Bu oturumu CONTEXT.md formatında özetle" |
| Karar al | "X konusunda karar verelim, seçenekleri listele" |
| Bağlamı kontrol et | "Şu ana kadar ne kararlaştırdık?" |
| Utku'ya mesaj bırak | "CONTEXT.md'ye Utku için bir not ekle: ..." |

---

## Önemli Notlar

- `CONTEXT.md`'yi her oturum başında **pull et** (GitHub sayfasını yenile)
- Push etmeyi unutma — Utku senin güncellemelerini oradan okuyacak
- Çakışma olursa (ikisi aynı anda düzenlerse) elle birleştirin

---

## Repo Linki

👉 https://github.com/utkuco/HK-deneme
