# Higgsfield Studio

**Claude Code + Higgsfield ile görsel/video üretim skill'leri** — [Avenox](https://www.youtube.com/@Avenoxai)

Videoda ekranda gördüğün her şeyin tarifi burada: görsel üretimi, Seedance
loop'ları, sunum sitesi — hepsi skill olarak, kendi projende aynen
çalıştırabileceğin hâlde.

> Bu repo Higgsfield sponsorlu videonun ekidir; skill'ler ve notlar kendi
> üretim hattımızdan çıktı, olduğu gibi paylaşıyorum.

## Skill'ler

| Skill | Ne yapar |
|---|---|
| [`higgsfield-generate`](skills/higgsfield-generate/) | Higgsfield üzerinden görsel / video / 3D / ses üretimi — GPT Image 2, Seedance, Nano Banana, Marketing Studio |
| [`higgsfield-soul-id`](skills/higgsfield-soul-id/) | Yüz/kimlik tutarlılığı — kendi karakterini eğitip her üretimde aynı yüzü koru |
| [`higgsfield-product-photoshoot`](skills/higgsfield-product-photoshoot/) | Ürün fotoğraf çekimi setleri |
| [`higgsfield-marketplace-cards`](skills/higgsfield-marketplace-cards/) | Pazaryeri ürün kartları |
| [`video-prompt-builder`](skills/video-prompt-builder/) | Seedance için retention-odaklı video planı: brief → klip klip prompt → birleştirme planı |
| [`sunum-sitesi`](skills/sunum-sitesi/) | **"Sıfırdan" tarzı sunum sitesi** — koyu temalı tek sayfa video eşlikçisi; görseller GPT Image 2, loop'lar Seedance, hepsi Higgsfield'dan |

## Kurulum

1. Skill'leri Claude Code'un gördüğü yere kopyala:

```bash
git clone https://github.com/avenoxai/higgsfield-studio.git
cp -R higgsfield-studio/skills/* ~/.claude/skills/
```

(Tek projede kullanacaksan `~/.claude/skills/` yerine projenin
`.claude/skills/` klasörü de olur.)

2. Higgsfield bağlantısı — iki yol, ikisi de çalışır:
   - **CLI:** `higgsfield-generate` skill'i CLI'ı kendisi kurar; ilk seferde
     `higgsfield auth login` ister.
   - **MCP:** Higgsfield'ın MCP bağlayıcısını Claude'a ekle (claude.ai →
     Connectors, veya Claude Code'da `claude mcp add`).

3. Claude Code'a konuş: *"şu görseli üret"*, *"bu fotoğrafı 5 saniyelik
   loop videoya çevir"*, *"bu video için sunum sitesi yap"* — ilgili skill
   kendiliğinden devreye girer.

## Notlar

- [`notlar/seedance-uretim-notlari.md`](notlar/seedance-uretim-notlari.md) —
  Seedance'in sahada öğrenilen davranışları: kuyruk, faturalama, loop mührü.
- [`notlar/oyun-reklami-promptlari.md`](notlar/oyun-reklami-promptlari.md):
  çekim yapmadan üretilen oyun reklamının bütün promptları, sırasıyla: tarz
  kilidi, referans paketi, Türkçe replikler, müzik, Seedance sahneleri, maliyet.

## Örnek çıktı

"Sıfırdan." serisinin bölüm sitesi bu boru hattıyla üretildi:
[avenoxai.github.io/sifirdan/site](https://avenoxai.github.io/sifirdan/site/) ·
[repo](https://github.com/avenoxai/sifirdan)

> Kredi/fiyat bilgileri değişkendir; güncel rakamları kendi hesabından doğrula.
