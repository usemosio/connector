---
name: gerak
description: "Use when choreographing motion for a Mosio composition: picking moves (snap, squash and stretch, smear, overshoot, anticipation, iris or mask wipe, morph, follow-through), setting durations, eases and staggers, keeping direction continuous across cuts, and structuring the GSAP code so every frame is a pure function of timeline time (gerak, animasi, timing, easing, transisi terasa kaku, gerakan murahan). Pair with get_composition_guide."
license: "MIT dan Apache-2.0; lihat bagian Diadaptasi dari"
metadata:
  author: Mosio
  version: "0.1.0"
  sources: "motion2code@0f00328, motionmaxxing@8c8ec0f, lottiefiles-motion-design@f9a8a04, hyperframes@908f2d0"
---

# Kriya gerak

Inti kerajinan gerak untuk komposisi Mosio: kosakata gerak, timing dan easing, hukum arah antar potongan, dan susunan kode di timeline. Pakai setelah storyboard disetujui dan sebelum menulis tween. Aturan teknis di `get_composition_guide` tetap yang berlaku; skill ini mengisi "bagaimana gerak yang bagus" di dalam batas itu.

## 1. Kosakata gerak

Kenali dulu gerak apa yang diminta adegan, lalu pakai resepnya. Kebiasaan buruk agen: satu `power1.inOut` untuk semuanya, sehingga terasa mengambang.

| Gerak              | Artinya                                                     | Di timeline Mosio                                                                                                                                                                                                                                      |
| ------------------ | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Snap               | Benda pindah ukuran atau posisi dalam 1-2 frame lalu diam   | `back.out(2)` 0,15-0,25 s, atau `tl.set` ke 90% lalu settle 3-10% dalam 7-14 frame                                                                                                                                                                     |
| Squash and stretch | Benda gepeng saat menumbuk, memanjang saat melesat          | saat kontak `scaleX: 1.2, scaleY: 0.8` selama 2-3 frame, lalu kembali ke 1; luas tetap (`scaleY ≈ 1/scaleX`, selisih paling banyak 10%); `transformOrigin` di titik kontak (dasar gelas, bawah kotak)                                                  |
| Smear              | Benda memanjang searah laju pada frame tercepat, tanpa blur | 1-3 frame `scaleX` 1,3-1,6 di sepanjang arah gerak; untuk arah miring, bungkus dengan wrapper yang diputar ke sudut gerak. Murah untuk render CPU, beda dengan blur                                                                                    |
| Overshoot          | Melewati sasaran sedikit lalu kembali                       | `back.out(1.4-1.7)`, lebih baik terlambat dan kecil (4-8%) daripada memantul keras; paling banyak satu per adegan                                                                                                                                      |
| Elastic settle     | Bergoyang beberapa kali sebelum diam                        | `elastic.out(1, 0.35)` hanya untuk benda ilustrasi (maskot, stiker, ikon produk) yang mendarat; satu kali per video, tidak untuk teks atau harga                                                                                                       |
| Anticipation       | Ancang-ancang berlawanan arah sebelum gerak utama           | 0,1-0,35 s, besarnya 10-20% gerak utama (`power2.in`, mis. gepeng atau mundur 3 frame), lalu gerak utama `power3.out`; hanya pada gerak kunci                                                                                                          |
| Iris / mask wipe   | Adegan baru terbuka lewat lingkaran atau tirai              | `clipPath: "circle(0px at 50% 60%)"` ke radius yang melewati sudut terjauh (hitung `Math.hypot` ke 4 sudut, kali 1,05); membuka `power2.out`, menutup `power2.in`, 0,3-0,5 s. Tirai: `inset(100% 0% 0% 0%)` ke `inset(0%)`. Adegan lama tetap di bawah |
| Morph chain        | Satu bentuk berubah jadi bentuk berikutnya sambil bergerak  | path SVG dengan struktur sama: `attr: { d }`; beda struktur: `MorphSVGPlugin`; lingkaran ke kotak bundar: tween `borderRadius` atau `rx`. Gelembung chat jadi tombol, titik jadi logo                                                                  |
| Follow-through     | Bagian yang ikut tertinggal lalu berhenti belakangan        | anak (label, bayangan, pita harga) 2-4 frame (0,05-0,15 s) di belakang induknya, berhenti 0,1-0,2 s lebih lambat, ease berbeda                                                                                                                         |
| Hard stop          | Gerak berhenti tegas pada punchline                         | `power4.out` masuk ke diam; tidak ada yang bergerak 0,3-0,6 s selain latar                                                                                                                                                                             |

Masuk dari besar lebih berwibawa daripada tumbuh dari nol: logo atau kata pembuka mulai 1,4-3x lalu mengecil ke ukuran akhir (`snap` atau `expo.out`, 0,5-0,8 s). Tumbuh dari kecil hanya bila tumbuhnya adalah isi (kartu yang jadi adegan berikutnya); itu pun mulai dari 0,6-0,9, bukan 0. Pakai "mulai besar" sekali per video, bukan di setiap adegan.

## 2. Timing dan easing

**Mendarat melambat, pergi mempercepat.** Masuk: `expo.out` atau `power3.out`, sebagian besar jarak habis di 4-8 frame pertama lalu ekor panjang. Keluar: `power2.in` atau `power3.in`, berakhir pada kecepatan puncak tepat saat potongan. Durasi keluar sekitar 65-75% durasi masuk. Ease `inOut` untuk benda yang berpindah di dalam layar, bukan untuk masuk atau keluar.

Durasi umum di 30 fps (1 frame = 0,033 s):

| Gerak                              | Frame          | Detik          |
| ---------------------------------- | -------------- | -------------- |
| Snap, potong keras, tekanan tombol | 2-3            | 0,07-0,1       |
| Opacity sebagai pembantu masuk     | 4-6            | 0,13-0,2       |
| Gerak biasa (kata, kartu, ikon)    | 10-12          | 0,33-0,4       |
| Kata judul, kartu utama            | 12-18          | 0,4-0,6        |
| Logo, produk hero, angka besar     | 18-24          | 0,6-0,8        |
| Drift latar                        | seluruh adegan | `ease: "none"` |

Gerak yang jauh butuh sedikit lebih lama (jarak 2x kira-kira 1,3x waktunya); lebih dari sepertiga layar tanpa perubahan arah atau kecepatan terasa seperti slide.

**Stagger.** Huruf 1-2 frame (0,03-0,05 s), kata 2-4 frame (0,06-0,12 s), tumpukan penutup (nama, ajakan, kontak) sekitar 8 frame (0,25 s). Total satu gelombang paling lama 0,5 s. Jeda tidak harus seragam: celah yang sedikit tidak rata terasa buatan tangan, dan celah terbesar diletakkan sebelum kata kunci. Durasi tiap anggota sekitar 2x jedanya, supaya ekornya bertumpuk.

**Tanpa frame mati.** Setiap frame punya satu hal yang hidup: latar yang melayang pelan (sesuai guide), garis berjalan, angka yang berdetak. Tapi latar bernapas bukan pengganti isi: adegan yang selesai masuk dan masih sisa 2 detik berarti butuh cerita (kata kedua, harga, detail produk), bukan goyangan. Jeda yang dirancang boleh: 0,3-0,75 s diam sebelum momen besar (harga muncul, logo mendarat) membuat momen itu terasa.

**Waktu baca.** Teks yang harus dibaca diam minimal 1,2 detik; frasa sekitar 20 huruf butuh 1,2 detik total. Paling banyak 7 kata di layar. Penutup (ajakan, nama usaha, kontak) diam minimal 2 detik.

**Satu sistem gerak per video.** Pilih satu kepribadian dan pakai konsisten:

| Kepribadian | Cocok untuk                     | Durasi gerak | Ease utama               | Overshoot                  |
| ----------- | ------------------------------- | ------------ | ------------------------ | -------------------------- |
| Ceria       | jajanan, mainan, kedai kekinian | 0,25-0,4 s   | `back.out(1.6)`          | 8-15%, satu pegas terlihat |
| Premium     | klinik, properti, skincare      | 0,5-0,8 s    | `expo.out`, `power2.out` | 0%                         |
| Rapi        | SaaS lokal, jasa B2B, kursus    | 0,3-0,5 s    | `power3.out`             | 0-3%                       |
| Energik     | flash sale, promo 12.12         | 0,15-0,35 s  | `expo.out`, potong keras | 10-20%                     |

Lalu kunci: satu ease untuk sekitar 80% gerak, tiga durasi (cepat, standar, lambat), satu pola masuk, satu aksen warna dengan satu makna, paling banyak satu pegas yang kelihatan. Jangan campur gaya blur: di Mosio pilih tajam (tanpa blur, ganti dengan smear dan potong keras) atau blur kecil di potongan saja (lihat skill `transisi`).

## 3. Hukum vektor antar potongan

Cara adegan A keluar menentukan cara adegan B masuk:

1. **Sumbu sama.** x tetap x, y tetap y, skala tetap skala.
2. **Arah sama, tidak dicerminkan.** Untuk skala, arah adalah tanda perubahannya: membesar berarti maju, mengecil berarti mundur. Keluar mengecil lalu masuk tumbuh dari kecil adalah pelanggaran paling umum.
3. **Kecepatan cocok.** Pakai ease cermin dengan jarak dan durasi sama: keluar `power4.in`, masuk `power4.out` (dua belahan satu `power4.inOut`).
4. **Potong di tengah gerak.** Adegan A masih melaju saat dipotong, adegan B sudah melaju di frame pertamanya. Diam dulu baru potong, atau mulai dari diam setelah potong, adalah ketukan mati.

**Arus.** Satu video punya satu arah dominan (bawaan: ke kiri, seperti membalik halaman berikutnya). Semua potongan biasa memakainya. Arah lain disimpan untuk makna:

| Vektor                                   | Makna                                                  |
| ---------------------------------------- | ------------------------------------------------------ |
| Arus (ke kiri)                           | lanjut ke ketukan berikutnya                           |
| Ke atas                                  | kesimpulan atau penawaran yang naik ke atas sebelumnya |
| Zoom maju (membesar)                     | masuk lebih dalam ke ide yang sama                     |
| Zoom mundur (masuk dari besar, mengecil) | kedatangan: harga, produk, penutup                     |

Dua potongan berturut-turut dengan arah berlawanan terbaca seperti salah. Ganti arah butuh sebab yang kelihatan (ketukan, tumbukan, klik) atau batas babak.

**Pembawa.** Mata mengikuti benda. Potongan terkuat menyerahkan satu benda melewati potongan pada posisi dan kecepatan yang sama: produk yang terus meluncur, gelembung chat yang jadi tombol, harga yang terbang ke kartu. **Sebab-akibat:** efek dimulai di frame penyebabnya, bukan "sesaat kemudian"; benda besar memantul lebih lambat dari benda kecil.

Tulis buku arah di storyboard (di chat), satu baris per potongan: `2,8 s pembuka ke produk: keluar kiri, masuk kiri, cut-the-curve, pembawa: gelas`. Kalau baris tidak cocok, perbaiki rencananya, bukan easing-nya. Resep dan kode transisinya ada di skill `transisi`.

## 4. Arsitektur kode

Satu file, satu timeline yang di-pause, dan semua yang terlihat pada waktu t hanya bergantung pada t.

- **Posisi waktu mutlak** dari storyboard (`tl.to(..., 2.8)`), bukan rantai `"+=0.2"`: mengubah satu ketukan tidak boleh menggeser semua ketukan sesudahnya. Konstanta waktu adegan di atas (`const S2 = 2.8`).
- **Keadaan awal dari timeline.** `from`/`fromTo` merender keadaan awal saat timeline dibangun. `tl.set(el, {...}, 0)` tidak dirender di frame 0 kecuali diberi `immediateRender: true`; pakai itu untuk menyembunyikan adegan yang muncul belakangan.
- **Acak ber-seed, dihitung sekali.** Posisi taburan, konfeti, atau bintang dibuat sebelum timeline dengan PRNG ber-seed, lalu ditween biasa. Tidak ada `Math.random()` atau fisika yang dihitung per langkah; pantulan dan pegas pakai ease bentuk tertutup (`elastic.out`, `CustomBounce`).
- **Satu grup per benda.** Gambar setiap benda di sekitar titik asalnya sendiri (tengah atau titik kontak), lalu gerakkan grupnya. Lapisan berurutan: `#latar`, `#dunia`, `#benda`, `#teks`. Satu adegan dibungkus satu wrapper yang bisa digerakkan seperti kamera; teks tidak di-zoom terpisah dari kameranya.
- **Palet sebagai konstanta.** Tiga warna di `:root` (`--latar`, `--utama`, `--teks`), turunan dengan `color-mix()`. Tidak ada warna baru per elemen.
- **Komentar per adegan dengan rentang waktunya**, supaya bisa diubah tanpa membaca semuanya.
- **Bagian prosedural** (banyak titik, angka) boleh lewat satu objek jam: `tl.fromTo(jam, {t: 0}, {t: D, duration: D, ease: "none", onUpdate: () => gambar(jam.t)}, 0)`, dengan `gambar(t)` yang menghitung ulang dari nol setiap kali.
- **Panjang timeline** sama dengan `data-duration`: tutup dengan `tl.set({}, {}, DURASI)` bila tween terakhir selesai lebih awal.

```js
const DURASI = 15,
  S2 = 2.8,
  S3 = 6.2;
function mulberry32(a) {
  return () => {
    a |= 0;
    a = (a + 0x6d2b79f5) | 0;
    let t = Math.imul(a ^ (a >>> 15), 1 | a);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}
const rnd = mulberry32(12);
const tl = gsap.timeline({ paused: true });
tl.set(["#adegan2", "#adegan3"], { autoAlpha: 0, immediateRender: true }, 0);

// Adegan 1 (0-2,8 s): gelas jatuh (gravitasi, jadi ease-in), gepeng saat mendarat, lalu settle
tl.fromTo("#gelas", { y: -1100 }, { y: 0, duration: 0.42, ease: "power3.in" }, 0.15)
  .to("#gelas", { scaleX: 1.2, scaleY: 0.83, duration: 0.07, ease: "power2.out" }, 0.57)
  .to("#gelas", { scaleX: 1, scaleY: 1, duration: 0.6, ease: "elastic.out(1, 0.35)" }, 0.64)
  .fromTo("#bayangan", { scaleX: 0.4 }, { scaleX: 1, duration: 0.3, ease: "power3.out" }, 0.6); // follow-through

// Taburan gula: posisi dari seed, dihitung sekali
document.querySelectorAll(".butir").forEach((el, i) => {
  tl.fromTo(
    el,
    { x: 0, y: 0, opacity: 1 },
    {
      x: (rnd() - 0.5) * 520,
      y: -180 - rnd() * 260,
      opacity: 0,
      duration: 0.7,
      ease: "power2.out",
    },
    0.6 + i * 0.012,
  );
});

// ... adegan 2 (2,8-6,2 s) dan 3 (6,2-15 s), potongan sesuai buku arah
tl.set({}, {}, DURASI);
window.__timelines = window.__timelines || {};
window.__timelines["main"] = tl;
```

## Periksa sebelum preview_composition

- Setiap gerak punya peran (masuk, keluar, drift, angka) dan ease yang sesuai perannya; tidak ada satu ease untuk semua.
- Overshoot dan pegas hemat: paling banyak satu pegas terlihat per video.
- Setiap potongan cocok dengan buku arah: sumbu, arah, kecepatan, dan dipotong di tengah gerak.
- Teks diam cukup lama untuk dibaca; penutup diam minimal 2 detik.
- Tidak ada `Math.random`, timer, atau animasi CSS; posisi waktu mutlak; panjang timeline sama dengan `data-duration`.

Untuk resep siap pakai, baca `hyperframes/hyperframes-animation/rules-index.md` (mis. `rules/spring-pop-entrance.md`, `rules/svg-path-draw.md`) dan blueprint seperti `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md` lewat `skill_library`. Contoh gaya Mosio yang sudah memakai pola ini: `kinetic-type`, `product-hero`, `flash-sale` (`get_composition_guide` dengan `style`).

## Diadaptasi dari

- [lovisdotio/motion2code](https://github.com/lovisdotio/motion2code) (MIT, © Lovis Odin), commit `0f00328`: `skills/motion2code/SKILL.md`, `skills/motion2code/references/motion-vocabulary.md`, `skills/motion2code/references/animation-architecture.md`.
- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana), commit `8c8ec0f`: `SKILL.md`, `references/motion.md`.
- [LottieFiles/motion-design-skill](https://github.com/LottieFiles/motion-design-skill) (MIT, © LottieFiles), commit `f9a8a04`: `skills/motion-design/SKILL.md`, `skills/motion-design/reference/timing-easing-tables.md`, `skills/motion-design/director/choreography.md`, `skills/motion-design/director/disney-principles.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen dan kontributor HyperFrames), commit `908f2d0`: `.claude/skills/motion-doctrine/SKILL.md`.

Perubahan oleh Mosio: diterjemahkan ke bahasa Indonesia, diringkas, dan disatukan menjadi satu panduan untuk timeline GSAP Mosio yang di-pause (tanpa audio, tanpa CDN). Dibuang: skrip studi video dan render (Python, ffmpeg, Playwright), runtime dan ease ukur khusus motionmaxxing, blur shutter dan smear berbasis blur, Three.js, sinkron suara, berkas ledger dan skrip Seam Gate; larangan "idle wobble" dilunakkan agar sesuai aturan latar hidup Mosio, dan elastic dibatasi untuk benda ilustrasi. Ditambah: tabel kepribadian untuk jenis UMKM, catatan `immediateRender` untuk keadaan awal, aturan waktu baca dan penutup 2 detik dari guide, serta contoh kode Mosio. Teks lisensi lengkap: `skills/licenses/` di repo Mosio, atau NOTICE.md di samping skill ini pada plugin.
