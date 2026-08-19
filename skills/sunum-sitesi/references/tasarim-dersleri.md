# Tasarım dersleri — gerçek şikayetlerden damıtıldı

Bunlar teori değil; Sıfırdan bölüm 1 sitesinde izleyici/creator geri
bildirimiyle tek tek ödenmiş dersler.

## Koyu tema tokenları

Sıcak füme + ivory — soğuk gri değil:

```css
body.dark {
  --paper:      #262624;   /* zemin — sıcak füme */
  --paper-2:    #2F2E2B;   /* kart zemini */
  --ink:        #EDE8DC;   /* ana metin — ivory */
  --ink-panel:  #171614;   /* koyu panel içi */
  --sand:       #57503F;   /* ikincil vurgu */
  --rule: 1px solid rgba(237,232,220,.13);
}
```

- Koyu tema **varsayılan** (teknik kitle koyu sever), `L` tuşu light.
- İkincil metinler için `#A69C8A` / `#B8B0A0` bandı — saf gri ölü durur.

## İllüstrasyon = kağıt plaka

Açık zeminli illüstrasyonlar koyu temada karartılMAZ; kart içinde "baskı
plakası" gibi durur:

```css
body.dark main img { mix-blend-mode: normal !important; }
```

Çarpma (multiply) modu açık temada zarif durur ama koyu temada görseli
yutar — spesifik seçicilerle kaçan görseller kalabilir, `!important`
buradaki nadir meşru kullanımdır.

## Zemin deseni

Düz koyu zemin "slop" hissi verirse: temayla uyumlu seamless desen üret,
üstüne koyu perde bindir — uzaktan düz koyu, yakından doku:

```css
body.dark.has-pattern {
  background-image:
    linear-gradient(rgba(38,38,36,.72), rgba(38,38,36,.72)),
    url(../assets/pattern-dark.png);
  background-size: auto, 460px auto;
}
```

Perde opaklığı ayar meselesidir: .9 = görünmez, .6 = göz yorar. .72
civarından başla.

## Boşluk ve yoğunluk

- **Boş alan taraması yap:** her bölümü tek ekranda gör; amaçsız boşluk
  varsa ya içerik büyür ya kutu küçülür. "Ferah" ile "boş" farkı: boşluk
  bir şeyi çerçeveliyorsa ferah, çerçevelemiyorsa boş.
- Sol ray dar (≈188px), liste bölümleri kompakt: satır yüksekliğini ve
  dikey padding'i kıs, görseli büyüt.
- İlk taslak HEP fazla geniş/seyrek çıkar; %25-50 sıkılaştırma turunu
  baştan planla.

## Okunurluk tuzakları

- **Rozet/etiket renkleri literal yaz** (`#FFD23E on rgba(10,9,7,.88)`),
  tema token'ı kullanma — token koyu temada ters düşüp görünmez rozet
  bırakır (video süre rozetinde ödenmiş ders).
- Uzun paragraf koyu temada açıktakinden zor okunur: satır genişliğini
  65ch altında tut, `line-height ≥ 1.6`.
- Süslü ayraç/dekor görselleri koyu temada filtre ister:
  `grayscale(1) invert(.92) sepia(.35); opacity:.2` bandı iyi başlangıç —
  yoksa mavi hale artefaktı verir.

## Hareket disiplini

- Hero'da sinema, gövdede sükunet: bölüm içinde sadece scroll-reveal +
  sayaç + loop. Her şey oynarsa hiçbir şey oynamıyordur.
- `prefers-reduced-motion` ve `?capture` (ekran kaydı) her animasyonu
  kapatabilmeli.
