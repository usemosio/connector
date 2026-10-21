---
name: brief
description: "Brand intake, a short creative brief, and a second-by-second storyboard, all in chat, before any composition HTML is written. Use at the start of every new Mosio video, when the request is vague or missing facts (price, contact, size, duration), or when the user wants to plan or revise scenes (bikin video, brief, storyboard, konsep video, rencana adegan, naskah video, ubah adegan)."
license: "MIT dan Apache-2.0; lihat bagian Diadaptasi dari"
metadata:
  author: Mosio
  version: "0.1.0"
  sources: "motion-graphics-skills@229022e, video-spec-builder@9e73275, hyperdirector@220fa2a, hyperframes@908f2d0"
---

# Brief dan storyboard

Skill ini mengubah "buatkan video" menjadi brief singkat dan storyboard per detik di chat, sebelum satu baris HTML ditulis. Ini langkah 1 dan 2 dari alur di `get_composition_guide`; semua aturan teknis di sana tetap berlaku dan menang kalau ada yang bertentangan.

## Urutan kerja

1. Panggil `get_composition_guide` dulu. Bagian "Brand kit workspace" berisi nama usaha, warna utama, warna latar, dan kontak. Itu sudah jawaban: jangan ditanyakan ulang, cukup ditampilkan di brief. Yang tertulis "(belum diisi)" masuk daftar pertanyaan.
2. Baca pesan pengguna dan ambil semua yang sudah terjawab (produk, harga, platform, durasi, contoh yang disukai). Jangan minta pengguna mengulang.
3. Tanyakan yang masih kurang dalam **satu batch, paling banyak 5 pertanyaan**, lalu tunggu.
4. Tulis brief dan storyboard (format di bawah), lalu tanya: "Sudah pas, atau ada yang diubah?"
5. Setelah setuju: pilih gaya, panggil `get_composition_guide` dengan `style`, tulis HTML, panggil `preview_composition`.

Langsung mulai dari pertanyaan atau brief. Tanpa pembukaan panjang, tanpa menjelaskan skill ini.

## Yang wajib ada sebelum storyboard

| Kolom | Jawaban yang diterima | Contoh |
| --- | --- | --- |
| Penawaran | satu hal yang dijual atau diumumkan, dengan angka asli | "Es kopi susu gula aren, Rp18.000" |
| Untuk siapa | satu orang yang spesifik, dan di mana ia menonton | "karyawan kantor sekitar Kuningan, scroll Reels saat makan siang" |
| Ajakan | satu tindakan | "Pesan lewat WhatsApp" |
| Kontak | persis seperti yang dipakai pelanggan | nomor WA, @akun, nama toko Shopee, alamat singkat |
| Ukuran | dari tempat unggah (tabel Ukuran di guide) | Reels dan TikTok: 9:16 |
| Durasi | angka detik | 15 detik |
| Rasa | paling banyak 3: hangat, premium, ceria, mendesak, tenang, tegas, modern | "hangat, ceria" |

Ukuran dan durasi boleh kamu sarankan (bawaan 9:16, 10-20 detik) asal ditandai sebagai saran. Harga, angka, kontak, alamat, jam buka, testimoni, dan rating hanya dari pengguna atau brand kit.

## Pertanyaan: satu batch, singkat

Pemilik UMKM sibuk dan menjawab dari HP. Satu pesan, bernomor, maks 5, tiap pertanyaan bisa dijawab satu baris. Pertanyaan faktual (ukuran, durasi) diberi pilihan dengan saranmu di depan. Pertanyaan kreatif (rasa, pesan inti) dibiarkan terbuka dengan satu contoh, supaya jawabannya tidak kamu arahkan.

Contoh untuk "bikinin video buat kedai kopi saya":

```
Biar videonya pas, 5 hal dulu:
1. Menu apa yang mau ditonjolkan, dan harganya berapa?
2. Siapa yang paling sering beli? (misal: mahasiswa sekitar kampus)
3. Setelah nonton, orang harus ngapain: chat WA, datang ke kedai, atau pesan di GoFood?
4. Diunggah di mana? Saran saya Reels/TikTok, vertikal 15 detik.
5. Kesan yang dicari, maks 3 kata (misal: hangat, santai)?
Nama usaha dan warna saya ambil dari brand kit: Kopi Senja, kuning dan biru gelap.
```

Jawaban kosong: tanyakan sekali lagi. Masih kosong: jangan ditebak. Tulis `[belum ada]` di brief, dan bagian itu tidak tampil di video (penutup tanpa nomor lebih baik daripada nomor karangan).

## Dorong sampai level adegan

Kata sifat dan perkiraan belum jawaban. Ubah jadi keputusan yang bisa digambar dengan menawarkan dua pilihan yang terbayang di kepala, plus akibatnya. Cukup satu putaran tambahan; jangan menginterogasi.

| Pengguna bilang | Balas dengan |
| --- | --- |
| "Yang kekinian aja" | "Kekinian yang mana: kata besar bergantian cepat seperti promo sneakers, atau putih bersih dengan satu produk besar dan banyak ruang kosong?" |
| "Yang mewah" | "Latar gelap, aksen emas, huruf Playfair yang masuk pelan. Atau krem terang dengan foto produk besar. Yang pertama lebih dramatis, yang kedua lebih kalem." |
| "Sekitar 15 detikan" | "Saya kunci 15 detik: 4 adegan, masing-masing 3 sampai 4 detik. Oke?" |
| "Buat semua orang" | "Siapa yang paling sering beli? Satu saja, nanti bahasanya saya sesuaikan." |
| "Biar viral" | "Kalau berhasil, orang yang nonton melakukan apa: chat WA, datang ke toko, atau klik toko Shopee?" |
| "Masukin semua menu" | "15 detik muat 3 menu yang terbaca. Mana 3 andalan? Sisanya bisa jadi video kedua." |
| "Pokoknya ada diskon" | "Diskonnya berapa, dari harga berapa, dan sampai kapan?" |
| "Terserah kamu" | Ambil keputusan sendiri, tulis di bagian "Saran saya" dengan alasan satu baris. Fakta tetap tidak boleh ditebak. |

**Pesan inti**: satu kalimat, maks 7 kata, bisa jadi judul poster ("Kopi gula aren asli, Rp18.000"). Pengguna memberi tiga pesan? Pilih satu bersama dia; dua lainnya video lain.

**Contoh dari brand lain** (video kompetitor, akun yang disukai): ambil geraknya saja. Logo, teks, dan warnanya tidak dipakai. Warna hanya dari hex yang diketik pengguna atau brand kit; dari tangkapan layar yang buram, tanyakan, jangan perkirakan hex-nya.

## Cek gabungan

Sebelum menulis storyboard, baca semua jawaban sekaligus dan cari masalah yang baru muncul kalau digabung. Sampaikan dalam satu kalimat beserta usulnya.

- 6 detik tapi 4 produk: usul satu produk, atau 15 detik.
- 1:1 tapi penutup berisi alamat, nomor, dan jam buka: usul nomor WA saja.
- Rasa "tenang" untuk flash sale: usul "tegas" atau pisahkan jadi dua video.
- 16:9 untuk status WhatsApp: usul 9:16.
- Harga dan persen diskon tidak cocok (Rp100.000 jadi Rp70.000 disebut "diskon 50%"): tanyakan mana yang benar.

## Format brief

Tulis pendek. Pisahkan yang berasal dari pengguna atau brand kit dari saranmu, supaya koreksi gampang.

```
Brief
- Penawaran: Es kopi susu gula aren, Rp18.000
- Untuk: mahasiswa sekitar kampus, nonton Reels sore hari
- Pesan inti: Kopi gula aren asli, Rp18.000
- Ajakan: Pesan lewat WhatsApp
- Kontak: [dari brand kit]
- Ukuran: 9:16, 1080×1920
- Durasi: 15 detik
- Rasa: hangat, santai
Dari kamu dan brand kit: penawaran, untuk, ajakan, kontak, warna.
Saran saya (perlu konfirmasi): 9:16 15 detik karena untuk Reels; gaya product-hero karena satu produk dengan harga.
```

## Format storyboard (presisi per detik)

Aturan:
- 3-6 adegan. Patokan: 6-8 detik 3 adegan, 10-15 detik 4-5 adegan, 20-30 detik 5-6 adegan.
- Detik ditulis satu angka desimal dan bersambung tanpa celah: adegan berikut mulai tepat di detik akhir adegan sebelumnya. Jumlahnya sama dengan durasi, dan nanti sama dengan `data-duration`.
- Adegan 1 adalah hook, 0-1,5 detik: gerak besar, teks pendek, langsung menarik. Bukan logo sendirian, bukan judul yang muncul pelan dari layar kosong.
- Adegan isi 2-4 detik, satu ide per adegan, dan setiap adegan punya satu kejadian (sesuatu berubah antara awal dan akhirnya). Teks yang harus dibaca diam setidaknya 1,2 detik.
- Adegan terakhir berisi ajakan, nama usaha, dan kontak, diam setidaknya 2 detik, lalu keluar di 0,5 detik terakhir.
- Teks layar ditulis persis seperti yang akan tampil, maks 7 kata sekaligus.
- Gerak: kata kerja dan rasa easing ("menghentak masuk", "dihitung naik lalu memantul", "terungkap dari balik garis"). Transisi: cara pindah ke adegan berikut (wipe, mask, zoom, geser, kata keluar sebelum kata baru masuk). Jangan sekadar muncul dan hilang.

Contoh (ilustratif; harga dan kontak harus dari pengguna):

| # | Detik | Teks layar | Gerak | Transisi |
| --- | --- | --- | --- | --- |
| 1 | 0,0-1,5 | Ngantuk jam 2 siang? | kata menghentak satu per satu, besar ke normal | kata keluar ke atas |
| 2 | 1,5-4,5 | Es Kopi Susu Gula Aren | gelas SVG naik dari bawah, latar gradien bergeser pelan | wipe diagonal warna utama |
| 3 | 4,5-8,0 | Cuma Rp18.000 | angka dihitung naik, mendarat dengan pantulan kecil | zoom keluar dari angka |
| 4 | 8,0-11,5 | Gula aren asli, bukan sirup | dua baris terungkap dari balik garis, kata "asli" diberi aksen | geser ke kiri |
| 5 | 11,5-15,0 | Pesan via WA · Kopi Senja · [kontak] | tombol masuk, lalu diam; fade keluar 14,5-15,0 | selesai |

Di bawah tabel tulis: "Total 15,0 detik. Sudah pas, atau ada yang diubah?"

## Persetujuan dan revisi

- Revisi bukan persetujuan. Setelah mengubah brief atau storyboard, tampilkan versi baru dan tanyakan lagi.
- "Langsung buat aja": lewati menunggu persetujuan (sesuai guide), tapi tetap tulis brief dan storyboard ringkas sebagai keputusan dengan alasan satu baris, supaya pengguna bisa mengoreksi di pratinjau. Fakta yang belum ada tetap ditanyakan atau dibuang dari video.
- Mengubah satu adegan: ubah adegan itu saja dan geser detik sesudahnya supaya totalnya tetap.
- Beberapa ukuran: storyboard sama, tata letak disusun ulang per ukuran. Tawarkan dulu, karena setiap ukuran memotong kredit sendiri.

## Serah ke komposisi

- Detik awal tiap adegan menjadi posisi waktu di timeline, misalnya `tl.addLabel("adegan3", 4.5)` lalu tween adegan 3 memakai `"adegan3"` atau `"adegan3+=0.2"`. Durasi timeline sama dengan total storyboard.
- Pilih gaya dari `COMPOSITION_STYLES` yang paling dekat dengan storyboard: satu produk dan harga `product-hero`, diskon `flash-sale`, slogan `kinetic-type`, menu `menu-board`, testimoni `chat-testimoni`, angka `angka-capaian`, aplikasi `peluncuran-aplikasi`, jasa iklan `dasbor-iklan`, layar lebar `explainer-isometrik`.
- Skill lanjutan: `iklan` untuk iklan berbayar Meta/TikTok, `promo` untuk diskon dan harga coret, `short-form` untuk retensi 9:16. Ritme kata besar: `skill_library` path `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md`.

## Diadaptasi dari

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills), commit `229022e`: `skills/brand-intake/SKILL.md`, `skills/motion-brief-writer/SKILL.md`.
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub), commit `9e73275`: `SKILL.md`, `references/workflow-0-1.md`, `references/question-bank.md`, `references/scene-breakdown.md`, `references/pacing-rules.md`, `templates/video-spec-template.md`.
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors), commit `220fa2a`: `hyperdirector/prompts/02-intake-brief.md`, `hyperdirector/prompts/03-storyboard-generator.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen dan kontributor HyperFrames), commit `908f2d0`: `skills/hyperframes/references/brief-contract.md`, `skills/hyperframes/references/intent-interview.md`, `skills/hyperframes/references/storyboard-format.md`.

Perubahan oleh Mosio: diterjemahkan dan diringkas ke bahasa Indonesia, disatukan menjadi satu alur di chat (tanpa file brand.md, MOTION.md, video-spec.md, BRIEF.md, atau JSON), pertanyaan dipadatkan menjadi satu batch maks 5 untuk UMKM, dan brand kit workspace dari `get_composition_guide` dipakai lebih dulu. Bagian suara, TTS, 3D, shader, pencarian aset, dan skor 4 fase dibuang; aturan durasi dan teks disesuaikan dengan guide Mosio, contoh diganti konteks UMKM Indonesia. Teks lisensi lengkap: `skills/licenses/` di repo Mosio, atau NOTICE.md di samping skill ini pada plugin.
