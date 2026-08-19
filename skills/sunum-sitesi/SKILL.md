---
name: sunum-sitesi
description: |
  Build a cinematic single-page dark presentation site for a video,
  talk or course episode — hero with animated panorama, section rail
  navigation, illustrated glossary cards, and living Seedance loops.
  All visuals are generated through Higgsfield (GPT Image 2 for stills,
  Seedance for 5s loops) so one tool covers the whole pipeline.
  Use when: "sunum sitesi yap", "video için site", "interaktif sunum",
  "presentation site", "episode site", "ders sitesi", or when the user
  wants an on-screen companion site for a video like the Avenox
  "Sıfırdan" series. Plain HTML/CSS/JS — no build step, no framework.
argument-hint: "[konu/video] [bölüm sayısı] [stil notu]"
---

# Sunum Sitesi — video eşlikçisi tek sayfa site

Bir video/ders/konuşma için ekranda gösterilecek sinematik, koyu temalı, tek
sayfalık sunum sitesi üretir. Avenox "Sıfırdan." bölüm 1 sitesinin damıtılmış
tarifi: aynı süreç, herkesin kendi konusuyla tekrar edebileceği şekilde.

**İlke:** framework yok, build yok. `index.html + css/ + js/ + assets/` —
`python3 -m http.server` ile açılır, herhangi bir hosting'e (GitHub Pages
dahil) olduğu gibi atılır.

## Süreç — 6 adım

### 1 · İçerik envanteri
Videonun/dersin omurgasını çıkar: bölümler (6-10 arası ideal), her bölümün tek
cümlelik derdi, ekranda *gösterilmeye* değer parçalar (liste mi, zincir mi,
sözlük mü, tablo mu). Site senaryoyu tekrar etmez; konuşurken arkada duran
görsel çapa üretir. Bkz. `references/site-iskeleti.md`.

### 2 · İskelet
Tek `index.html`: sol sabit ray (bölüm navigasyonu, scroll-spy), sağda akan
bölümler. Her bölüm `<section id>` + numara + kicker + başlık kalıbında.
Scroll-reveal ve sayaçlar IntersectionObserver ile. Hazır kalıplar ve rail
ölçüleri `references/site-iskeleti.md` içinde.

### 3 · Tasarım katmanı
Koyu tema varsayılan (tokenlar `references/tasarim-dersleri.md`), `L` tuşu
light'a çevirir. İllüstrasyonlar koyu zeminde "kağıt plaka" olarak durur —
karartılmaz. Boşluk disiplini, okunurluk ve rozet kuralları aynı dosyada;
hepsi gerçek izleyici şikayetlerinden çıkmış derslerdir.

### 4 · Görseller — Higgsfield GPT Image 2
Tüm illüstrasyonlar tek stil kilidiyle üretilir: bir stil çapası paragrafı
yaz, HER prompt'a aynen yapıştır, ilk üretilen en iyi kareyi referans görsel
olarak sonraki üretimlere ekle. Batch üretim, adlandırma ve tekrar eden
karakter tarifi `references/gorsel-uretim.md` içinde.

### 5 · Loop'lar — Seedance
En değerli görselleri 5 sn / 720p sessiz loop videoya çevir (image-to-video).
Seedance kendi kendine loop KAPATMAZ — ffmpeg ping-pong mührü şart. Üretim
ayarları, mühür komutu ve doğrulama `references/loop-uretim.md` içinde.

### 6 · Entegrasyon
Loop mp4'leri `assets/loops/` altına aynı isimle koy; JS, HEAD isteğiyle mp4
var mı bakar, varsa `<img>`'yi sessiz videoya çevirir — mp4 yoksa PNG kalır
(zarif düşüş). Autoplay yarışları çözülmüş hazır kod `references/loop-uretim.md`
sonunda.

## Kontrol listesi (yayın öncesi)

- [ ] Her bölüm tek ekranda derdini anlatıyor mu? (scroll'suz test)
- [ ] Boş alan taraması: rail dahil hiçbir bölgede amaçsız boşluk yok
- [ ] Koyu temada TÜM metinler okunur (rozetler, süre etiketleri dahil)
- [ ] Loop'lar sekme arka plana alınıp dönünce devam ediyor
- [ ] `?capture` paramı animasyonları kapatıyor (ekran kaydı için)
- [ ] `python3 -m http.server` önbelleği: değişiklik görünmüyorsa `?v=2` ekle
