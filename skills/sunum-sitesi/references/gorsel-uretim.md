# Görsel üretim — Higgsfield GPT Image 2

Sitenin bütün illüstrasyonları Higgsfield üzerinden GPT Image 2 ile üretilir
(`higgsfield-generate` skill'i ya da Higgsfield MCP'si — ikisi de olur).
Amaç: 30-50 görsel, hepsi TEK elden çıkmış gibi.

## Stil kilidi — tek paragraf, her prompt'a aynen

Önce stil çapası paragrafını yaz. Örnek kalıp (kendi karakterin/paletinle
doldur):

> Flat editorial illustration, warm ivory paper background, restrained
> ink/halftone texture, thin black contour lines, muted palette of
> [2-3 renk], single [karakter tarifi] as the recurring hero, calm
> composition with generous negative space, no text, no watermark.

Kurallar:

1. Bu paragraf **her** prompt'un başına aynen yapıştırılır. Kısaltma,
   "önceki gibi" deme — model önceki diye bir şey hatırlamaz.
2. İlk turdan en iyi kareyi seç, sonraki üretimlere **referans görsel**
   olarak ekle (image input). Stil sürüklenmesini bu keser.
3. Tekrarlayan karakter varsa tarifini de kilitle: siluet, yüz ifadesi,
   uzuvlar, enerji — ve bir de **negatif liste** ekle ("NEVER: ...") ki
   model karakteri "iyileştirmesin".
4. Metin gerekiyorsa Türkçe karakterleri açıkça iste: "Turkish characters
   İ Ş Ğ Ü Ö Ç rendered correctly." Yine de kritik metni görselin içine
   değil HTML'e yaz — görsel içi metin en kırılgan parçadır.

## Batch üretim

- Görselleri konuya göre adlandır (`soz-agent.png`, `ic-harness.png`,
  `hero-panorama.png`) — bu adlar loop dosyalarıyla eşleşecek.
- Bağımsız görselleri paralel üret; darboğaz üretim süresi, istek sayısı
  değil.
- Paralel üretimde çıktı tekrarı görülebilir: bitince `md5` ile eş dosya
  taraması yap, eşleşenleri tek tek yeniden üret.

## Görsel türleri (Sıfırdan b1'de işe yarayan set)

| Tür | Adet | Not |
|---|---|---|
| Hero panorama | 1 | Geniş sahne; sonra Seedance ile canlanacak |
| Bölüm başı sahne | bölüm başına 1 | Karakter + bölümün metaforu |
| Sözlük/kavram kartı ikonu | terim başına 1 | Aynı çerçeve, aynı ölçek |
| Küçük ikonlar (zincir adımları vb.) | 4-8 | Tek konsept, tek nesne |

## Koyu temada görseller

İllüstrasyonları koyu üretme. Açık "kağıt plaka" olarak üret, koyu zeminde
kart içinde göster (`mix-blend-mode: normal` — çarpma modu koyu temada
görseli yutar). Zemin deseni istiyorsan ayrı bir seamless pattern üret ve
üstüne koyu perde bindir (bkz. tasarim-dersleri.md).
