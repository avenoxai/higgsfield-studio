# Seedance üretim notları — sahadan

Gerçek üretimde (13+ loop, tek gecede) ödenmiş dersler.

## Ayarlar

- Sunum loop'u için tatlı nokta: **5 sn / 720p / sessiz**. Kredi maliyeti
  düşük, ekranda fark yok.
- Girdi görselini `start image` olarak ver; prompt'ta sahneyi yeniden tarif
  etme, sadece hareketi söyle. Kamera kilitli ("camera locked") — kayan
  kamera loop'u öldürür.

## Kuyruk davranışı

- Hesap bazında eşzamanlılık sınırlı (~2-3 iş). Hepsini birden gönderme:
  fazlası hızlı-başarısız düşer. 2'şerli gönder, bitmesini bekle.
- **Başarısız işler faturalanmaz** (bakiyeden düşmez) — yine de bakiyeyi
  `balance` ile ara ara doğrula.
- Backend arızası saatlerce sürebilir. Deneme başına sınır koy (3 deneme,
  sonra ele) ve elenenleri listede tut — media yüklemeleri hazır durur,
  arıza geçince aynı id'lerle yeniden gönderirsin.
- Arayüz/istemci bir "preset" önerisiyle gelirse ve istemediysen açıkça
  reddet — sessiz kabul, işi bambaşka bir stile kaçırabilir.

## Loop gerçeği

Seedance son kareyi ilk kareye BAĞLAMAZ; "loop" istesen de, end frame'e
start görselini koysan da. Ping-pong mührü şart:

```bash
ffmpeg -i in.mp4 -filter_complex \
  "[0:v]split[a][b];[b]reverse[r];[a][r]concat=n=2:v=1[v]" \
  -map "[v]" -an -movflags +faststart out.mp4
```

Doğrulama: son→ilk kare farkı, ardışık kare farklarının medyanını
aşıyorsa sarma görünür demektir — kesimi kaydır ya da yeniden üret.
Ayrıntılar `skills/sunum-sitesi/references/loop-uretim.md` içinde.
