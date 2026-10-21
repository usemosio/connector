---
name: sutradara-video
description: "Write a promo video as an HTML composition that Mosio renders, starting from example styles: kinetic type, product hero, flash sale, menu board, chat testimonial, numbers and charts. Use when the user wants their own look, names a style, or no Mosio template fits."
---

# Sutradara video Mosio

Kamu merancang videonya sendiri sebagai satu file HTML (komposisi Hyperframes): tata letak, tipografi, warna, dan gerak. Mosio memeriksanya, menampilkan pratinjau bergerak di kartu, lalu merendernya menjadi MP4 di server. Kamu sutradaranya; Mosio studio dan kameranya.

## Alur
1. Pahami tujuannya: apa yang dijual atau diumumkan, untuk siapa, dan di mana diunggah (lihat Ukuran; bawaan vertikal 1080×1920). Jangan mengarang harga, alamat, kontak, atau jam buka; tanyakan kalau belum ada.
2. **Brief dan storyboard dulu, sebelum menulis HTML.** Tulis di chat, singkat:
   - Brief: produk atau penawaran, untuk siapa, ajakan, kontak, ukuran, dan durasi.
   - Storyboard: 3–6 adegan, masing-masing dengan rentang detik, teks di layar, dan geraknya.

   Tanyakan apakah sudah pas, lalu revisi di tahap ini (gratis dan cepat). Mulai menulis HTML setelah pengguna setuju, kecuali pengguna meminta langsung dibuatkan.
3. Pilih gaya yang cocok dan baca contoh kodenya (`get_composition_guide` dengan `style`). Contoh itu titik awal, bukan cetakan: ubah teks, warna, ritme, bentuk, dan adegannya sesuai usaha pengguna dan storyboard yang disetujui.
4. Tulis HTML lengkap lalu panggil `preview_composition`. Kalau ditolak, baca `details` (kode lint, pesan, saran perbaikan), perbaiki, kirim lagi.
5. Pengguna menonton pratinjau di kartu dan menekan "Buat video" sendiri (memotong kredit sebesar durasi). Revisi lewat chat: ubah HTML-nya dan panggil `preview_composition` lagi.
6. Di aplikasi tanpa kartu (terminal, skrip): setelah pengguna setuju, panggil `render_composition`, lalu `get_status` sampai `done`.

## Ukuran
| Format | `data-width` × `data-height` | Untuk |
| --- | --- | --- |
| Vertikal 9:16 (bawaan) | 1080 × 1920 | Reels, TikTok, Shorts, status WhatsApp |
| Portrait 4:5 | 1080 × 1350 | Feed Instagram, iklan |
| Kotak 1:1 | 1080 × 1080 | Feed Instagram dan Facebook, marketplace |
| Lebar 16:9 | 1920 × 1080 | YouTube, website, layar toko |

- Satu brief bisa jadi beberapa ukuran: tulis satu komposisi per ukuran dengan urutan adegan dan durasi yang sama. Susun ulang tata letaknya untuk tiap ukuran (misalnya teks di samping visual pada 16:9), jangan sekadar diskalakan atau dipotong.
- Setiap ukuran dirender terpisah dan masing-masing memotong kredit sebesar durasinya. Tawarkan dulu sebelum membuat beberapa ukuran sekaligus.
- Video dirender tanpa suara. Kalau pengguna ingin musik, sarankan menambahkannya di aplikasi tempat video diunggah (Reels, TikTok).

## Pustaka sutradara
`skill_library` dimulai dengan skill sutradara Mosio di `mosio/` (bahasa Indonesia, sudah sesuai aturan di bawah): `mosio/brief/` (brief dan storyboard), `mosio/iklan/` dan `mosio/promo/` (iklan dan promo diskon), `mosio/gerak/` (bahasa gerak), `mosio/review/` (cek sebelum dan sesudah pratinjau), serta `short-form`, `kinetic`, `transisi`, `angka`, `chat`, `peluncuran`, `format` (beberapa ukuran), `logo`, dan `slideshow-foto`. Isinya juga skill pihak ketiga yang bisa kamu baca: Hyperframes resmi (kontrak komposisi `hyperframes/hyperframes-core/SKILL.md`, aturan gerak, blueprint adegan, dan transisi di `hyperframes/hyperframes-animation/`, arahan kreatif di `hyperframes/hyperframes-creative/`, alur promo di `hyperframes/product-launch-video/`), motion graphic berbahasa Indonesia (`bang-motion/`), arah gerak dan komposisi 9:16 (`motion-design/`), struktur video pendek dan hitung mundur (`short-form/`), kinetic typography, GSAP, dan copywriting. Untuk efek siap pakai (teks kinetik, transisi, overlay, kartu UI, grafik), cari dulu di registry Hyperframes dengan `search_registry` (kata kunci bahasa Inggris) lalu `get_registry_item`, dan tempel ke komposisimu daripada membuat dari nol. Baca bagian yang relevan sebelum merancang. Kalau sebuah skill menyuruh menjalankan CLI, memakai media, atau framework lain, ikuti catatan di indeksnya. Aturan di bawah ini yang berlaku di Mosio.

## Aturan teknis (wajib, dicek sebelum render)
- Satu dokumen HTML. Elemen akar: `<div id="root" data-composition-id="main" data-start="0" data-duration="15" data-width="1080" data-height="1920" data-fps="30">`. Durasi yang disarankan 6–30 detik, paling lama 60 detik. `html` dan `body` seukuran `data-width` × `data-height` (misalnya 1080×1920 px), begitu juga `<meta name="viewport">`; `#root` `width: 100%; height: 100%; overflow: hidden` (ukuran kanvas dari `data-width`/`data-height`, jangan di-hardcode di `#root`).
- Semua gerak ada di satu timeline GSAP yang di-pause dan didaftarkan; renderer memutarnya frame demi frame:
  ```js
  const tl = gsap.timeline({ paused: true });
  // ... tween dengan posisi waktu eksplisit: tl.from("#judul", {...}, 0.4)
  window.__timelines = window.__timelines || {};
  window.__timelines["main"] = tl;
  ```
- Dilarang: `setTimeout`, `setInterval`, `requestAnimationFrame`, animasi atau transition CSS, `Date.now()`, `new Date()`, `Math.random()`. Semuanya tidak ikut timeline sehingga hasil render berbeda dari pratinjau. Butuh acak? Pakai PRNG kecil dengan seed tetap (mulberry32). Butuh berulang? `repeat` dengan angka tertentu, jangan `-1`.
- Aset: `<script src="vendor/gsap.min.js"></script>` (GSAP 3.14), plugin GSAP `vendor/<Nama>.min.js` (CustomEase, CustomBounce, CustomWiggle, DrawSVGPlugin, EasePack, Flip, MorphSVGPlugin, MotionPathPlugin, Physics2DPlugin, PhysicsPropsPlugin, ScrambleTextPlugin, SplitText, TextPlugin; daftarkan dengan `gsap.registerPlugin`), `vendor/mosio.js` (opsional: `Mosio.readableOn(hex)` memberi warna teks hitam atau putih yang terbaca), font di bawah, SVG dan CSS inline, gambar `data:` kecil. URL CDN GSAP (jsdelivr, cdnjs, unpkg) diganti salinan lokal otomatis. Selain itu tidak ada URL http(s), iframe, video, atau audio: render berjalan tanpa internet.
- Teks ditulis langsung di HTML. Escape `&` dan `<` di dalam teks. Jangan pakai `<br>` di teks isi (biarkan membungkus dengan `max-width`); pengecualian hanya judul pendek yang sengaja satu kata per baris.
- Jangan memberi `transform` awal di CSS pada elemen yang di-tween GSAP di properti yang sama; atur keadaan awal di timeline (`fromTo`, `xPercent`/`yPercent`).
- Render memakai CPU: hindari WebGL, `filter: blur` besar di elemen layar penuh, `backdrop-filter`, dan ratusan elemen bergerak. Gradien, SVG, transform, opacity, dan clip-path murah.
- Gerakkan dengan transform (`x`, `y`, `xPercent`, `scale`, `rotation`) dan `opacity`, bukan `top`/`left`/`width`.

## Font
Dua pilihan, sama di pratinjau dan render:
- **Google Fonts**: tautkan seperti biasa, misalnya `<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Anton&family=Inter:wght@400;800&display=swap">`. Mosio mengunduh huruf latinnya saat pemeriksaan dan menyimpannya bersama proyek. Pilih keluarga yang memang ada di Google Fonts.
- **Font bawaan** di bawah, tanpa unduhan. Deklarasikan yang dipakai:
```css
@font-face { font-family: "Bebas Neue"; src: url("vendor/BebasNeue-400.woff2"); font-weight: 400; }
```

| Keluarga | Ketebalan | File | Kesan |
| --- | --- | --- | --- |
| Bebas Neue | 400 | `vendor/BebasNeue-400.woff2` | judul tegak rapat, kinetic type, olahraga, promo |
| Archivo Black | 400 | `vendor/ArchivoBlack-400.woff2` | tebal dan lantang: diskon, harga, angka besar |
| Poppins | 500 | `vendor/Poppins-500.woff2` | teks isi yang ramah dan modern |
| Poppins | 800 | `vendor/Poppins-800.woff2` | judul geometris yang modern |
| Playfair Display | 700 | `vendor/PlayfairDisplay-700.woff2` | elegan: kafe, kue, kecantikan, butik |
| Playfair Display | 400 italic | `vendor/PlayfairDisplay-400-italic.woff2` | aksen miring yang elegan |
| Space Grotesk | 500 | `vendor/SpaceGrotesk-500.woff2` | teknologi, gadget, jasa digital |
| Space Grotesk | 700 | `vendor/SpaceGrotesk-700.woff2` | judul teknologi |
| Caveat | 700 | `vendor/Caveat-700.woff2` | tulisan tangan: rumahan, catatan, sentuhan personal |
| Noto Sans | 400 | `vendor/NotoSans-Regular.ttf` | netral, cadangan untuk semua teks |
| Noto Sans | 700 | `vendor/NotoSans-Bold.ttf` | netral tebal |

Pakai paling banyak dua keluarga per video: satu untuk judul, satu untuk isi.

## Tata letak dan zona aman
- Vertikal 9:16: jauhkan teks penting dari 250 px teratas, 420 px terbawah (tombol dan caption Reels/TikTok), dan 80 px di kiri dan kanan.
- Portrait 4:5 dan kotak 1:1: sisakan 80 px di setiap tepi.
- Lebar 16:9: sisakan 120 px di kiri dan kanan serta 80 px di atas dan bawah; teks paling lebar sekitar setengah layar, sisanya untuk visual.
- Judul 120–260 px, teks pendukung 54–80 px, terkecil 40 px. Paling banyak 7 kata di layar sekaligus.
- Kontras cukup: teks di atas warna merek pakai `Mosio.readableOn` atau pilih manual yang jelas terbaca.

## Bahasa gerak (yang membuat video terasa modern)
- Struktur 15 detik: pembuka 0–1,5 detik yang langsung menarik (gerak besar, teks pendek), 2–4 adegan isi (satu ide per adegan, 2–4 detik), penutup dengan ajakan, nama usaha, dan kontak yang diam setidaknya 2 detik.
- Pindah adegan dengan transisi: wipe atau mask (`clipPath`), geser, zoom, atau kata keluar sebelum kata baru masuk. Jangan sekadar muncul dan hilang.
- Easing: masuk `expo.out` atau `power3.out`, keluar `power2.in`, pantulan `back.out(1.6)` seperlunya. Durasi gerak 0,35–0,8 detik. Stagger kata atau huruf 0,03–0,08 detik.
- Reveal bertopeng: bungkus teks dengan `overflow: hidden`, lalu `from(span, { yPercent: 110 })`. Ini ciri khas motion yang rapi.
- Latar selalu sedikit hidup (gradien atau bentuk melayang pelan, garis berjalan), tapi teks yang sedang dibaca diam setidaknya 1,2 detik.
- Angka naik: tween objek `{ v: 0 }` dengan `onUpdate` yang menulis teks berformat (misalnya `Rp${Math.round(o.v).toLocaleString("id-ID")}`).
- Warna: warna utama merek, warna latar, dan satu aksen. Turunan warna dengan `color-mix()`.
- Selesaikan dengan rapi: diam di penutup lalu fade atau scale keluar di 0,5 detik terakhir.

## Sebelum mengirim
- Semua teks muat dan terbaca di zona aman, tidak ada yang terpotong.
- Setiap elemen yang muncul belakangan punya keadaan awal dari `from`/`fromTo` di timeline, bukan disembunyikan dengan CSS.
- Durasi timeline sama dengan `data-duration`.

## Contoh per gaya
Baca contohnya sebelum menulis; jadikan titik awal, bukan cetakan.

- [Kinetic type](gaya/kinetic-type.html): Kata-kata besar masuk bergantian mengikuti ritme. Cocok untuk slogan, ajakan, dan pengumuman.
- [Produk utama](gaya/product-hero.html): Produk tampil megah dengan cahaya, nama, dan harga yang dihitung naik. Cocok untuk jualan satu produk.
- [Flash sale](gaya/flash-sale.html): Persen diskon meledak, pita berjalan, dan hitung mundur. Cocok untuk promo terbatas waktu.
- [Papan menu](gaya/menu-board.html): Menu dan harga muncul berurutan seperti papan menu kafe. Cocok untuk usaha makanan dan minuman.
- [Testimoni chat](gaya/chat-testimoni.html): Ulasan pelanggan sebagai gelembung chat yang diketik, lalu bintang dan nama usaha.
- [Angka dan grafik](gaya/angka-capaian.html): Angka naik dan diagram batang tumbuh. Cocok untuk pencapaian, perbandingan, dan bukti sosial.
- [Peluncuran aplikasi](gaya/peluncuran-aplikasi.html): Masalah dicoret, jendela aplikasi meluncur, kursor mengklik, dan angka naik. Cocok untuk aplikasi, SaaS, dan produk digital.
- [Dasbor iklan](gaya/dasbor-iklan.html): Metrik turun lalu naik, kartu platform, dan garis grafik yang tergambar. Cocok untuk jasa iklan, agensi, dan layanan digital.
- [Explainer isometrik (16:9)](gaya/explainer-isometrik.html): Panggung isometrik: masalah, beban kerja, lalu solusi dan grafik. Contoh layar lebar 1920×1080 untuk YouTube, website, dan layar toko.
