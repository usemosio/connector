---
name: review
description: "Use to critique a Mosio composition before calling preview_composition and again after seeing the preview or hearing the user's feedback: a never-ship list of AI-slop patterns, what good looks like, truth rules, the business-purpose-on-mute test, lazy defaults to question, and a pass/fail checklist to score every round (review, kritik, cek kualitas, kenapa videonya terasa murahan, sebelum render). Pair with get_composition_guide."
license: "MIT dan Apache-2.0; lihat bagian Diadaptasi dari"
metadata:
  author: Mosio
  version: "0.1.0"
  sources: "motionmaxxing@8c8ec0f, motion-video-kit@e4b1c00, design-motion-principles@4a9ca87, claude-motion-design@3d90d34, hyperframes@908f2d0"
---

# Review video sebelum dan sesudah pratinjau

Kritik diri yang dijalankan dua kali tiap putaran: sebelum memanggil `preview_composition` (pada storyboard dan kode) dan sesudah pratinjau tampil atau pengguna berkomentar. Aturan teknis tetap di `get_composition_guide` dan lint `preview_composition`; skill ini menilai apakah videonya bagus, jujur, dan menjual.

## Cara melihat

Lulus lint bukan berarti bagus. Versi yang bersih dan mulus tetap ditolak kalau terasa biasa, kosong, atau lambat. Nilailah seperti orang asing yang menonton sekali di HP, tanpa suara, sambil menggulir.

1. **Jangan menilai niat, nilai yang tampil.** Bila hasil `preview_composition` menyertakan gambar frame, lihat gambarnya. Bila tidak, telusuri timeline pada waktu kunci: frame 0, setiap potongan (0,1 s sebelum dan sesudah), tengah setiap adegan, dan 2 detik terakhir. Tanyakan: apa yang terlihat pada detik itu?
2. **Jelaskan setiap adegan dalam satu kalimat:** apa yang di layar, apa heronya, dan apakah frame itu lolos sebagai slide presentasi atau halaman web. Adegan yang tidak bisa dijelaskan dalam satu kalimat tidak punya hero; kalimat yang mulai dengan "kartu di atas warna polos" adalah frame slide.
3. **Empat cara melihat:** ukuran thumbnail (masih terbaca?), tutup logonya (masih cocok untuk pesaing?), tutup teksnya (masih ada gambar?), dan frame di tengah potongan (dua setengah keadaan bertumpuk, frame kosong, kilat putih?).
4. **Urutan perbaikan:** yang tidak benar atau rusak, lalu ide dan pesan, lalu sambungan antar adegan, lalu hierarki dan skala, lalu timing, terakhir finishing. Jangan menyetel easing selagi idenya belum jelas.
5. **Jangan mensertifikasi karya sendiri.** Ke pengguna, sebut apa yang diperiksa dan apa yang masih lemah; jangan menulis "sudah sempurna" atau "terverifikasi".

## Jangan pernah dikirim

Video yang memuat salah satu ini belum selesai. Hapus elemennya, jangan dibela.

1. **Chrome HUD:** nama usaha kecil di pojok sebagai label, label huruf kapital berjarak lebar, penghitung "01 / 04", timecode, progress bar, siku di sudut, label adegan di atas gambar.
2. **Tata letak slide:** blok judul rata kiri yang diam sementara isinya berganti, kicker kecil di atas judul, judul lalu isi, header dan footer, "judul kiri, HP kanan" yang diam.
3. **Kartu UI generik:** kartu bundar dengan titik status dan "label · Rp nilai", kartu "Selamat pagi, Kak", bar skeleton abu-abu, ikon emoji sebagai pengganti ikon asli.
4. **Copy slogan:** tiga kalimat satu kata ("Cepat. Murah. Terpercaya."), "Bukan sekadar X, ini Y", "Wujudkan impianmu", "Masa depan belanja", "Solusi terbaik untuk Anda".
5. **Statistik karangan:** dinding angka, slide angka (angka besar di tengah di atas label kecil) tanpa sumber dari pengguna, "terjual 10rb+" yang tidak pernah disebut.
6. **Semua elemen dengan fade-up yang sama:** empat elemen atau lebih dengan masuk identik (opacity dan y sama, durasi sama, ease sama) adalah tanda slop; begitu juga stagger yang sama di setiap daftar dan denyut atau glow yang berulang di banyak elemen.
7. **Halaman web kosong:** kartu kecil (sekitar 0,3 lebar layar) mengambang di latar polos, latar warna datar tanpa cahaya, bentuk, atau tekstur, satu benda kecil di tengah layar lebih dari 1,5 detik.
8. **Ending ala footer web:** logo, tagline, tombol, dan URL ditumpuk seperti kaki halaman, tanpa sebab dari cerita. Penutup Mosio tetap wajib (ajakan, nama usaha, kontak, diam minimal 2 detik), tapi ia ketukan terakhir cerita: datang dibawa benda atau gerak sebelumnya, ajakannya satu dan spesifik ("Pesan lewat WA"), tanpa tagline tambahan.
9. **Crossfade antar adegan** (dua adegan transparan bertumpuk) dan kilat putih atau hitam di potongan.

Catatan: motionmaxxing membatasi kartu penutup paling lama 1,4 detik. Di Mosio tidak berlaku: penutup diam minimal 2 detik supaya kontak sempat dibaca dan ditangkap layar.

## Seperti apa yang bagus

1. **Idenya milik usaha ini.** Diambil dari produk aslinya, cara menyajikan, bahan, suasana tempat, atau kalimat khas pemiliknya. Tutup logonya: kalau video tetap cocok untuk kedai sebelah, idenya dari kategori.
2. **Penonton melihat sesuatu terjadi.** Klaim diperagakan, bukan ditulis: es dituang, pesanan WA masuk lalu dikemas, sebelum dan sesudah. Satu kasus spesifik, nyata, dari pengguna.
3. **Setiap ketukan menyebabkan ketukan berikutnya.** Benda dibawa melewati potongan, arah gerak konsisten (lihat skill `gerak`).
4. **Penekanan dihemat.** Satu hero per frame, paling banyak tiga titik fokus; yang besar butuh yang kecil di sekitarnya; paling banyak seperempat ketukan adalah "intinya".
5. **Waktu bernapas.** Jeda sebentar sebelum yang penting, waktu baca pas, berhenti sebelum idenya habis.
6. **Satu dunia.** Satu logika cahaya, bahan, dan warna dari awal sampai akhir; latar adalah permukaan (gradien, cahaya, bentuk, tekstur), bukan swatch.
7. **Ada satu frame yang layak di-screenshot,** dan itu momen yang paling serius digarap.

Alasan penolakan yang sering didengar dari klien: "terlalu biasa, cuma gambar lalu teks", "banyak ruang terbuang", "bagian ini kelamaan", "jaraknya tidak rata", "pembukanya terlalu gelap" (untuk makanan, kesehatan, dan produk rumah tangga, pilih terang dan jernih).

## Aturan kejujuran

- Jangan mengarang harga, diskon, testimoni, nama pelanggan, rating bintang, jumlah terjual, jumlah pengikut, sertifikasi, garansi, alamat, jam buka, atau kontak. Semuanya dari pengguna; kalau belum ada, tanyakan atau biarkan kosong.
- Semua ilustrasi diberi label **"Contoh"** (atau "Ilustrasi") yang terbaca di layar: chat buatan, angka contoh, UI tiruan, ulasan contoh.
- Tidak meniru persis tampilan atau logo aplikasi lain (WhatsApp, Shopee, Tokopedia). Gaya generik yang mirip boleh.
- Klaim di teks harus benar untuk usaha ini ("dikirim hari ini" hanya kalau pengguna bilang begitu). Keterangan tentang video juga harus benar: jangan menyebutnya buatan AI. Mosio tidak memakai model AI untuk membuat gambar, video, atau suara; gerak dan tampilannya ditulis sebagai kode.

## Tujuan bisnis tanpa suara

Video Mosio selalu bisu. Penonton yang baru pertama kali melihat, tanpa suara dan di ukuran HP, harus bisa menjawab di akhir video:

1. Apa yang dijual, dan oleh siapa (nama usaha terlihat).
2. Satu alasan untuk peduli (rasa, harga, kemudahan, hasil).
3. Apa yang harus dilakukan sekarang: chat WA, datang ke alamat, klik link di bio, checkout di toko online.

Tes: tulis ulang pesan video hanya dari teks dan gambar yang tampil. Kalau jawabannya kabur atau butuh suara, video gagal, seberapa pun bagus geraknya.

## Kebiasaan malas yang perlu dipertanyakan

Ini bukan larangan. Masing-masing boleh kalau dipilih dengan alasan satu kalimat untuk usaha ini; yang salah adalah memakainya karena kebiasaan.

- Teks gradien (`background-clip: text`), garis aksen di tepi kiri kartu.
- Cyan di atas gelap, gradien ungu ke biru, aksen neon; `#000` atau `#fff` murni (tint ke arah warna aksen).
- "Premium pastiche": latar hampir hitam, glow radial, satu kata serif miring. Gelap karena mereknya gelap, atau karena gelap terasa mahal?
- Grid kartu yang identik, semua di tengah dengan bobot sama.
- Asosiasi pertama dari nama atau kategori: biji kopi melayang untuk kedai kopi, petir untuk "kilat", roket untuk "peluncuran".
- Semua fitur dari brief diberi adegan dengan bobot sama; tidak ada yang jadi "intinya".
- Keadaan, bukan kejadian: semuanya muncul lalu diam.
- Efek baru di setiap adegan untuk pamer teknik.
- Gaya rumah sebagai bawaan: kata raksasa terpotong, wipe lingkaran di setiap potongan, judul diketik dengan caret, titik yang jadi logo.

## Daftar periksa setiap putaran

Tandai setiap butir **Lulus** atau **Gagal**. Butir Kritis yang gagal harus diperbaiki sebelum pengguna menekan "Buat video".

| # | Butir | Tingkat |
| --- | --- | --- |
| 1 | Jujur: tidak ada harga, testimoni, angka, atau kontak karangan; ilustrasi berlabel "Contoh" | Kritis |
| 2 | Tanpa suara: apa yang dijual, oleh siapa, dan apa yang harus dilakukan jelas di akhir | Kritis |
| 3 | Penutup: ajakan, nama usaha, kontak diam minimal 2 detik, di zona aman, terbaca di ukuran HP | Kritis |
| 4 | Teks di zona aman, paling banyak 7 kata sekaligus, tidak terpotong atau bertabrakan, termasuk saat transisi | Kritis |
| 5 | Frame 0 sudah berupa komposisi atau sedang bergerak; tidak ada frame kosong lebih dari 3 frame dan tidak ada kilat putih di potongan | Kritis |
| 6 | Tidak ada satu pun butir "jangan pernah dikirim" | Penting |
| 7 | Satu hero per frame; dengan teks ditutup, setiap adegan masih menampilkan gambar | Penting |
| 8 | Potongan mengikuti hukum vektor (sumbu, arah, kecepatan, dipotong di tengah gerak), tanpa crossfade | Penting |
| 9 | Gerak per peran: masuk melambat, keluar mempercepat; tidak ada 4 elemen dengan masuk identik; paling banyak satu pegas terlihat | Penting |
| 10 | Waktu: teks dibaca minimal 1,2 detik; tidak ada frame yang beku total (latar pun diam) lebih dari 0,6 detik selain jeda yang dirancang dan penutup; panjang adegan bervariasi | Penting |
| 11 | Satu sistem: satu kepribadian gerak, satu aksen warna, paling banyak dua keluarga font | Peluang |
| 12 | Ada satu frame layak screenshot dan bisa disebut detiknya | Peluang |

Sebelum `preview_composition`: jalankan butir 1-10 pada storyboard dan kode (baca posisi waktu tween, ukuran teks, warna). Sesudah pratinjau: jalankan semuanya lagi, lalu bandingkan dengan putaran sebelumnya: setiap kegagalan lama ditandai Beres, Sebagian, atau Masih ada.

Laporkan ke pengguna singkat, misalnya: "Review putaran 2: 10 dari 12 lulus. Gagal: #5 (kilat putih di 6,2 s karena `#root` tanpa latar), #9 (lima kartu menu masuk dengan fade-up yang sama). Saya perbaiki dua itu dulu." Perbaiki paling banyak tiga kegagalan terberat per putaran, berurutan dari yang paling berdampak. Dua sampai tiga putaran biasanya cukup; berhenti ketika perbaikan tinggal kosmetik.

## Diadaptasi dari
- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana), commit `8c8ec0f`: `SKILL.md`, `references/slop.md`.
- [echris6/motion-video-kit](https://github.com/echris6/motion-video-kit) (MIT, © echris6), commit `e4b1c00`: `business-motion-film/SKILL.md`, `business-motion-film/references/quality-bar.md`, `business-motion-film/references/critic-prompts.md`.
- [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) (MIT, © Kyle Zantos), commit `4a9ca87`: `skills/design-motion-principles/workflows/audit.md`, `skills/design-motion-principles/references/anti-checklist.md`, `skills/design-motion-principles/references/audit-checklist.md`.
- [howseen-ai/claude-motion-design](https://github.com/howseen-ai/claude-motion-design) (MIT, © Howseen AI (Raphaël Aubry)), commit `3d90d34`: `skill/motion-design/SKILL.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen dan kontributor HyperFrames), commit `908f2d0`: `skills/hyperframes-creative/references/house-style.md` (salinan di `skill_library`: `hyperframes/hyperframes-creative/references/house-style.md`).

Perubahan oleh Mosio: diterjemahkan ke bahasa Indonesia, diringkas, dan digabung menjadi satu rutinitas review untuk alur `preview_composition` (tanpa audio, tanpa skrip render). Dibuang: gerbang yang dihitung skrip (look.py, lint.mjs, ffmpeg), kritikus sub-agen terpisah, NOTE.md dan berkas laporan HTML, pemeriksaan audio dan loudness, aturan kursor dan 3D, serta batas kartu penutup 1,4 detik yang diganti aturan Mosio (diam minimal 2 detik). Ditambah: contoh dan copy konteks UMKM, label "Contoh", tes tujuan bisnis tanpa suara versi Mosio, dan daftar periksa berskor dengan tingkat Kritis, Penting, Peluang. Teks lisensi lengkap: `skills/licenses/` di repo Mosio, atau NOTICE.md di samping skill ini pada plugin.
