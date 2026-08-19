# Site iskeleti

## Dosya yapısı

```
site/
  index.html
  css/style.css      # iskelet + tipografi + bölüm kalıpları
  css/site.css       # sayfaya özel katman (hero, özel bölümler)
  css/dark.css       # koyu tema katmanı — EN SON yüklenir
  js/app.js          # tüm etkileşim, tek dosya
  assets/            # PNG illüstrasyonlar
  assets/loops/      # aynı isimli mp4 loop'lar (opsiyonel)
```

Sıra önemli: `dark.css` en son gelir ki token'ları ezebilsin.

## Bölüm kalıbı

Her bölüm aynı ritimle açılır — numara, kicker, başlık:

```html
<section id="harita" class="rv">
  <div class="sec-head">
    <span class="sec-num">03</span>
    <span class="kicker">BU HAFTANIN YOLU</span>
  </div>
  <h2>Harita ve sözleşme</h2>
  ...
</section>
```

`rv` sınıfı scroll-reveal'a kaydolur. 6-10 bölüm ideal; daha fazlası rayda
sıkışır, daha azı "site" hissi vermez.

## Sol ray (navigasyon)

- Sabit (`position: fixed`), dar tutulur: **~188px** yeterli. İlk denemeler
  hep fazla geniş çıkar; ray içerik değil pusula.
- Linkler bölüm `id`'lerinden türetilir; scroll-spy IntersectionObserver ile
  (`threshold: 0.4`) aktif bölümü işaretler. Yeni bölüm eklenince nav'a
  otomatik girer — elle liste bakımı yapma.
- Aktif link koyu temada DÜZ vurgu rengi + koyu metin olmalı; yarı saydam
  vurgu, geçiş anında okunmaz kareler bırakır.

## Scroll-reveal ve sayaçlar

```js
var ro = new IntersectionObserver(function (es) {
  es.forEach(function (e) {
    if (e.isIntersecting) { e.target.classList.add('in'); ro.unobserve(e.target); }
  });
}, { threshold: 0.18, rootMargin: '0px 0px -40px 0px' });
document.querySelectorAll('.rv').forEach(function (el) { ro.observe(el); });
```

Sayı vurguları için `data-count` + easing'li countUp; `threshold: 0.6` ile
ancak gerçekten görününce oynar.

## Hero

Sinematik açılış üç parçadan oluşur:

1. **Panorama arka plan** — geniş tek illüstrasyon (`hero-bg`), hafif
   mouse-parallax (`--px/--py` custom property). `prefers-reduced-motion`
   ve `?capture` modunda kapat.
2. **Harf harf başlık** — başlığı `<span style="--i:N">` harflerine böl,
   CSS'te `animation-delay: calc(var(--i) * 40ms)`.
3. **Kademeli giriş** — kicker → başlık → stat satırı → cue, hepsi fadeUp
   ile artan gecikmede.

## Servis ve kayıt modu

- Geliştirme: `python3 -m http.server 8080` — ama önbelleğe dikkat:
  değişiklik görünmüyorsa URL'e `?v=2` ekle.
- Ekran kaydı için `?capture` query paramı: animasyonları ve parallax'ı
  kapatır, her kare temiz durur.
- `L` tuşu koyu/açık tema geçişi; `?light` ile açık başlar.
