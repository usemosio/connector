---
name: iklan
description: "Structure for paid performance video ads on Meta (Facebook, Instagram) and TikTok: a 0-3 s hook, context, payoff, proof, and a CTA held at least 2 s, with the hook message-matched to the CTA and the landing (WhatsApp chat, marketplace listing), sound-off readability, and A/B variants that change one variable. Use when the user wants an ad to run with budget (iklan, iklan FB, iklan IG, iklan TikTok, Meta Ads, boost postingan, konten iklan, A/B test, varian iklan, hook iklan)."
license: "MIT; lihat bagian Diadaptasi dari"
metadata:
  author: Mosio
  version: "0.1.0"
  sources: "ad-video-skills@0de0f2a"
---

# Video iklan berbayar

Skill ini untuk video yang akan dipasang dengan budget di Meta Ads atau TikTok Ads, di mana tiap detik dan tiap klik dibayar. Jalankan `brief` dulu (brief dan storyboard di chat), lalu pakai struktur di sini untuk mengisi adegannya. Aturan teknis tetap dari `get_composition_guide`.

## Dua aturan yang membuat iklan layak diuji

1. **Hook dan ajakan adalah satu pasangan.** Janji di 3 detik pertama harus dibayar oleh ajakan di akhir, dan oleh tempat orang mendarat setelah klik. Hook "Kemeja kusut tiap Senin?" ditutup "Chat WA, kemeja rapi besok pagi", bukan "Belanja sekarang".
2. **Satu varian, satu perubahan.** Kalau ingin menguji hook, hanya hook yang berbeda. Tata letak, warna, gerak, durasi, dan ajakan dibuat sama. Dua hal berubah sekaligus membuat pemenangnya tidak bisa dibaca.

## Anatomi iklan 15 detik

| Beat | Tugas | Detik (15 dtk) | Detik (30 dtk) |
| --- | --- | --- | --- |
| Hook | hentikan jempol: masalah, kejutan, atau janji | 0-2,5 | 0-3 |
| Konteks | buat penonton merasa masalahnya | 2,5-5 | 3-8 |
| Payoff | produk sebagai jawaban, satu manfaat jelas | 5-9 | 8-18 |
| Bukti | satu angka, demo, atau hasil yang nyata | 9-12 | 18-25 |
| Ajakan | satu tindakan, cocok dengan hook, diam ≥2 detik | 12-15 | 25-30 |

- Pemicu hook (kata kunci atau gambar utama) harus sudah terbaca sebelum detik 2. Penonton memutuskan lanjut atau geser dalam satu sampai dua detik.
- Iklan 6 detik (bumper): hook 0-2, payoff 2-4, ajakan 4-6. Konteks dan bukti dibuang.
- Patokan durasi: Feed Meta 6-15 detik, Reels dan Stories 8-15 detik, TikTok In-Feed 15-30 detik. Bawaan Mosio 10-20 detik.
- Hook yang paling berpengaruh ke biaya per hasil. Uji hook dulu, baru penawaran dan ajakan.

## Hook: satu kalimat, satu gambar

Di Mosio video dirender tanpa suara, dan kebanyakan orang menonton iklan dalam mode senyap. Hook terdiri dari dua bagian yang saling melengkapi, bukan mengulang:
- **Gerak visual** di 0-2 detik: produk jatuh ke frame, harga dicoret, chat masuk, kemeja kusut berubah rapi.
- **Teks layar**: klaimnya, maks 7 kata, besar, di zona aman.

Jangan biarkan teks hanya mendeskripsikan gambar ("Ini kemeja" di atas gambar kemeja). Gambar menunjukkan keadaannya, teks menyebut akibatnya.

### Jenis hook (contoh ilustratif, sesuaikan dengan fakta pengguna)

| Jenis | Bentuk | Contoh UMKM | Cocok saat |
| --- | --- | --- | --- |
| Masalah dulu | sebut masalah yang dirasakan | "Kemeja kusut tiap Senin pagi?" (laundry) | masalahnya terasa tapi jarang disebut |
| Pemutus pola | kalimat yang tidak disangka | "Jangan pesan katering ini dulu" | kategori ramai dan mirip-mirip |
| Celah penasaran | tahan jawabannya | "Bahan rahasia sambal kami: bukan cabai" | penonton skeptis, suka tahu |
| Manfaat langsung | hasil yang bisa diukur | "Laporan stok beres dalam 5 menit" (aplikasi kasir) | hasilnya jelas dan benar |
| Panggil langsung | sebut segmennya | "Yang jualan di Shopee, berhenti rekap manual" | segmen tajam |
| Pertanyaan | pertanyaan yang biasa diketik pembeli | "Cari kue ulang tahun di Depok?" | pencarian lokal |
| POV | situasi yang sangat spesifik | "POV: tanggal tua, lapar, dompet tipis" | penonton muda, produk murah |

Angka di hook (5 menit, 6 jam) hanya kalau pengguna memberikannya dan memang benar. Lebih banyak pola dan cara mendiagnosis hook yang gagal: `skill_library` path `copywriting/ad-creative/references/hook-system.md`.

## Message match: hook, ajakan, tempat mendarat

| Hook menjanjikan | Ajakan di video | Yang harus ada setelah klik |
| --- | --- | --- |
| "Kemeja rapi besok pagi" | "Chat WA, jemput hari ini" | admin WA siap menjawab soal jemput dan harga per kg |
| "Kue ulang tahun di Depok" | "Pesan di Shopee: [nama toko]" | listing kue ulang tahun, bukan halaman toko umum |
| "Gratis ongkir se-Jabodetabek" | "Klik, ongkir kami tanggung" | syarat gratis ongkir terlihat di listing |

- Nama produk, harga, dan promo di video harus sama persis dengan di WhatsApp atau listing marketplace. Beda sedikit saja membuat orang batal.
- Untuk iklan klik-ke-WhatsApp, sarankan pengguna menyiapkan pesan sapaan yang menyambung janji hook.
- Kontak dan nama toko diambil dari brand kit atau pengguna. URL tidak perlu ada di video kalau tombol iklan sudah mengarah ke sana; cukup ajakan yang jelas.

## Bukti yang jujur

Bukti paling kuat adalah yang bisa dicek: jumlah terjual, rating, testimoni asli, foto sebelum dan sesudah milik pengguna, atau demo produknya bekerja. Semuanya **hanya dari pengguna**. Tidak ada data? Pakai bukti yang tidak butuh angka: tunjukkan prosesnya (adonan, mesin cuci, layar aplikasi), atau lewati beat bukti. Gaya yang cocok: `chat-testimoni` untuk ulasan, `angka-capaian` untuk angka, `peluncuran-aplikasi` untuk demo aplikasi. Klaim kesehatan, kecantikan, dan keuangan dibatasi kebijakan iklan Meta dan TikTok: hindari janji hasil ("kulit putih 7 hari") dan sarankan pengguna mengecek kebijakannya.

## Terbaca tanpa suara

- Seluruh cerita harus jalan dalam mode senyap: hook, manfaat, dan ajakan ada sebagai teks layar.
- Teks pendukung 54-80 px, judul 120-260 px, kontras tinggi (pakai `Mosio.readableOn`).
- 9:16: zona aman dari guide (250 px atas, 420 px bawah, 80 px kiri dan kanan). Di TikTok, kolom ikon di sisi kanan menutupi sebagian layar (perkiraan sekitar 140 px, tergantung perangkat): taruh teks penting di tengah, jangan mepet kanan.
- Hook, produk, dan ajakan sebaiknya ada di kotak tengah 1080×1080, supaya versi 4:5 dan 1:1 tinggal disusun ulang tanpa memotong isi.
- Jangan menaruh musik atau voice-over di rencana. Kalau pengguna mau musik, tambahkan di Ads Manager atau editor TikTok setelah render.

## Varian untuk A/B test

Satu komposisi, satu bagian yang diganti. Tandai elemen yang diuji supaya varian berikutnya cukup mengganti teks itu:

```html
<!-- VARIAN: hook. Selain teks ini, semua sama antar-varian. -->
<h1 id="hook" class="hook">Kemeja kusut tiap Senin pagi?</h1>
...
<!-- VARIAN: ajakan (diuji di gelombang kedua) -->
<div id="ajak" class="ajak">Chat WA, kemeja rapi besok</div>
```

Urutan uji:
1. **Hook**: 3-5 hook berbeda jenis (dari tabel di atas) dengan badan dan ajakan yang sama.
2. **Penawaran atau manfaat**: hook pemenang, ganti janji utamanya.
3. **Ajakan**: hook dan penawaran pemenang, ganti kalimat ajakan (tetap cocok dengan hook).
4. **Ukuran**: iklan pemenang dalam 9:16, 4:5, 1:1.

Aturan Mosio untuk varian:
- Durasi dan timeline sama persis; hanya teks yang berubah. Cek hook terpanjang tetap muat (maks 7 kata, tidak terpotong di zona aman).
- Pratinjau tiap varian dengan `preview_composition`. Setiap render memotong kredit sebesar durasi, jadi tawarkan dulu jumlah varian dan ukurannya sebelum membuat semuanya.
- Beri judul yang menyebut variabelnya, misalnya "Laundry Kilat, hook 2: pemutus pola", supaya hasil di Ads Manager mudah dicocokkan.
- Kalau ajakan ikut diganti pada uji hook, pastikan pasangannya tetap cocok, dan catat bahwa uji itu menguji pasangan, bukan hook saja.

## Cek sebelum dikirim

- Pemicu hook terbaca sebelum detik 2; adegan pertama sudah bergerak sejak frame awal.
- Ajakan membayar janji hook, dan cocok dengan WhatsApp atau listing tujuan.
- Satu varian hanya berbeda di satu bagian.
- Semua angka, harga, testimoni, dan kontak berasal dari pengguna atau brand kit.
- Bisa dipahami penuh tanpa suara; teks di zona aman dan di kotak tengah.
- Penutup diam setidaknya 2 detik, durasi timeline sama dengan `data-duration`.

## Diadaptasi dari

- [iart-ai/ad-video-skills](https://github.com/iart-ai/ad-video-skills) (MIT, © iart.ai), commit `0de0f2a`: `skills/ad-creative-video/SKILL.md`, `skills/ad-creative-video/references/platform-specs.md`, `skills/ad-creative-video/references/batch-ad-pipeline.md`.

Perubahan oleh Mosio: diterjemahkan dan diringkas ke bahasa Indonesia, disesuaikan dengan aturan komposisi Mosio (satu file HTML, timeline GSAP yang di-pause, tanpa audio, tanpa CDN). Kode Remotion, parser CSV, dan skrip render batch dibuang dan diganti penanda varian di HTML serta pratinjau per varian; contoh hook diganti konteks UMKM Indonesia, ditambah message match ke WhatsApp dan marketplace serta aturan bukti yang jujur. Teks lisensi lengkap: `skills/licenses/` di repo Mosio, atau NOTICE.md di samping skill ini pada plugin.
