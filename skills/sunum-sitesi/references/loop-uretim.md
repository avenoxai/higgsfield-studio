# Loop üretimi — Seedance + ffmpeg mührü

Sitedeki görsellerin en değerlilerini 5 saniyelik sessiz loop videoya
çevirmek siteyi "canlı" hissettirir. Boru hattı: PNG → Seedance
image-to-video → ffmpeg ping-pong mührü → `assets/loops/`.

## Seedance ayarları

- **Girdi:** sitedeki PNG'nin kendisi (start image olarak).
- **Süre/çözünürlük:** 5 sn, 720p, ses yok. Sunum loop'u için fazlası israf.
- **Prompt:** sahneyi yeniden tarif etme; sadece *hareketi* söyle.
  "Subtle idle motion, gentle ambient drift, character breathes and blinks,
  camera locked, no cuts, no new elements." Kamera kilidi önemli — kayan
  kamera loop'u imkânsızlaştırır.
- Kuyruğu boğma: 2-3 eşzamanlı iş gönder, bitmeden yenisini yükleme.
  Başarısız işler genelde faturalanmaz ama kontrol et.

## Kritik gerçek: Seedance loop KAPATMAZ

Son kare ilk kareye dönmez — "loop" veya "end frame = start frame" istesen
de. Çözüm ping-pong mührü: videoyu sonuna gelince tersine oynat.

```bash
ffmpeg -i in.mp4 -filter_complex \
  "[0:v]split[a][b];[b]reverse[r];[a][r]concat=n=2:v=1[v]" \
  -map "[v]" -an -movflags +faststart out.mp4
```

10 sn'lik kusursuz döngü verir. Hareket "ileri-geri" hissettirmesin diye
Seedance prompt'unda tek yönlü büyük hareketlerden kaçın (yürüme, akan
yazı); nefes/salınım/ışık titremesi ping-pong'da doğal durur.

### Mühür doğrulaması

Sarma noktasının pürüzsüzlüğünü ölç: son kare ile ilk kare arasındaki fark,
ardışık kareler arası medyan farkın altındaysa mühür temiz. Pratik yöntem:
ffmpeg ile kareleri 160x90 griye küçült, ardışık kare farklarını ve
son→ilk farkını karşılaştır. Sarma farkı medyanın 1.5 katını aşıyorsa o
loop'u yeniden üret ya da kesim noktasını kaydır.

## Entegrasyon — zarif düşüşlü autoplay

`assets/loops/<isim>.mp4` varsa `<img src="assets/<isim>.png">` videoya
dönüşür; yoksa PNG kalır. Autoplay yarışları (sekme arka planda, geç yüklenme,
görünürlük) çözülmüş hâli:

```js
function ifVid(src, cb) {
  fetch(src, { method: 'HEAD' }).then(function (r) { if (r.ok) cb(src); })
    .catch(function () {});
}
var loopObs = ('IntersectionObserver' in window) ? new IntersectionObserver(function (es) {
  es.forEach(function (e) {
    var v = e.target;
    if (e.isIntersecting) { v.play && v.play().catch(function () {}); }
    else { v.pause && v.pause(); }
  });
}, { rootMargin: '120px' }) : null;
document.addEventListener('visibilitychange', function () {
  if (document.visibilityState !== 'visible') return;
  document.querySelectorAll('video').forEach(function (v) {
    var r = v.getBoundingClientRect();
    if (r.bottom > -120 && r.top < innerHeight + 120) v.play && v.play().catch(function () {});
  });
});
function armLoop(v) {
  v.addEventListener('canplay', function () { v.play && v.play().catch(function () {}); }, { once: true });
  if (loopObs) loopObs.observe(v); else v.play && v.play().catch(function () {});
}
function swapToLoop(img, src) {
  var v = document.createElement('video');
  v.src = src; v.muted = true; v.loop = true; v.autoplay = true;
  v.playsInline = true; v.setAttribute('playsinline', '');
  v.className = img.className; v.classList.add('loop');
  v.setAttribute('aria-label', img.alt || '');
  img.replaceWith(v);
  armLoop(v);
}
['ornek-gorsel-adi'].forEach(function (name) {
  var img = document.querySelector('img[src="assets/' + name + '.png"]');
  if (!img) return;
  ifVid('assets/loops/' + name + '.mp4', function (src) { swapToLoop(img, src); });
});
```

Püf noktaları:

- `muted + playsinline` olmadan mobil autoplay çalışmaz.
- `canplay` beklemeden `play()` çağırmak sessizce başarısız olur — yarışın
  kaynağı budur.
- `visibilitychange` dinleyicisi, sekme arka plandan dönünce duraklamış
  loop'ları uyandırır (tarayıcı gizli sekmede videoyu duraklatır; bu normal).
- Video, `<img>`'nin sınıflarını devralır — CSS'te ayrıca `video.loop`
  için köşe/gölge kuralı ekle.
