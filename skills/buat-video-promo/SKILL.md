---
name: buat-video-promo
description: "Buat video promosi (Reels, TikTok, status WhatsApp) untuk usaha kecil Indonesia dengan Mosio: promo produk, diskon, menu makanan, jasa, testimoni, pembukaan toko, pengumuman. Pakai saat pengguna minta video promosi atau konten jualan. Use when the user asks for a promotional video or sales content for a small business."
license: Proprietary
metadata:
  brand: Mosio
---

# Membuat video promosi dengan Mosio

Mosio membuat video promosi vertikal (Reels, TikTok, status WhatsApp) untuk usaha kecil di Indonesia dan merendernya di server. Ada dua cara, dan kamu boleh memakai keduanya:

- **Rancang sendiri (utama).** Kamu menjadi sutradara: tulis videonya sebagai komposisi HTML dengan gaya yang cocok, sehingga setiap video unik dan terasa modern. Baca `get_composition_guide` (aturan, font, bahasa gerak, brand kit, dan contoh kode per gaya), tulis HTML-nya, lalu `preview_composition`. Pengguna menonton pratinjaunya dan menekan "Buat video". Pakai cara ini kalau pengguna ingin hasil yang khas, menyebut gaya, atau tidak ada template yang pas.
- **Template siap isi (cepat).** Pilih template dan isi teksnya (alur di bawah). Cocok kalau pengguna memilih template dari galeri atau ingin cepat.

Pengguna cukup menceritakan usahanya dalam satu-dua kalimat; kamu yang menyusun naskah, teks layar, dan desainnya. Jangan meminta pengguna mengisi kolom satu per satu. Tanyakan hanya fakta yang tidak boleh dikarang (harga, alamat, kontak, jam buka).

## Alur template
Pengguna memilih lewat tampilan, bukan lewat daftar teks. Jangan menulis daftar template di chat dan jangan meminta pengguna memilih template lewat chat; di aplikasi yang mendukung, kartu interaktif yang menampilkan pilihannya. Hanya di aplikasi tanpa tampilan (misalnya terminal) ringkas pilihannya sebagai teks.

1. Panggil `list_templates` untuk membaca variabel dan batas panjangnya. Ini data untukmu; tidak perlu diringkas ke pengguna.
2. Kalau pengguna belum tahu mau video seperti apa atau bertanya gaya dan template apa saja yang ada, panggil `show_templates` (galeri bergerak berisi gaya rancangan dan template, bisa dibatasi `category`). Pilihan pengguna datang sebagai pesan berikutnya: gaya berarti kamu merancang videonya dengan gaya itu, template berarti alur template di bawah.
3. Kalau isinya sudah jelas (produk, harga, menu, info), langsung pilih template yang paling cocok:
   - jual produk dengan harga atau harga coret: `promo-produk`
   - potongan harga terbatas waktu: `diskon-kilat`
   - beberapa menu dan harga: `menu-fnb`; satu menu andalan: `menu-spesial`
   - jasa (laundry, servis, salon, les): `jasa-layanan`
   - ulasan pelanggan: `testimoni`; pembukaan toko atau cabang: `buka-baru`; info libur, jam buka, atau kabar lain: `pengumuman`

   Lalu panggil `preview_video` dengan id template lengkap (misalnya `promo-produk@1`) dan variabelnya, ditambah 2–3 template lain yang juga cocok di `alternatives` dengan teks yang disesuaikan ke variabel template itu. Ini gratis: tidak merender dan tidak memakai kredit, tapi memeriksa variabel dengan aturan yang sama. Pengguna melihat pratinjau bergerak, memilih desain dan warna, mengubah teks, lalu menekan "Buat video" sendiri; kartu memberi tahu kamu pilihannya dan id videonya.
4. Kalau pengguna meminta lewat chat (atau sudah jelas minta langsung dibuat), panggil `create_video` dengan variabel yang sama dan `idempotency_key` unik untuk permintaan itu. Ukuran bawaan 1080×1920.
5. `create_video` langsung mengembalikan id. Kalau kartu tidak tampil, panggil `get_status` setiap 5–10 detik sampai status `done` (biasanya kurang dari satu menit), lalu berikan link unduhannya. Kalau `failed`, sampaikan pesannya dengan bahasa sederhana.

## Menulis teksnya
- Bahasa Indonesia yang ramah dan singkat, seperti pemilik toko berbicara ke pelanggan. Tanpa tanda seru berlebihan, tanpa klaim yang tidak bisa dibuktikan ("termurah se-Indonesia").
- Patuhi batas panjang setiap variabel; teks yang terlalu panjang ditolak. Lebih pendek lebih baik terbaca di layar HP.
- Harga dalam format Rupiah: `Rp25.000`. Harga coret hanya kalau memang ada harga lama.
- Jangan mengarang harga, alamat, nomor kontak, atau jam buka. Kalau belum diberikan, tanyakan dulu, atau kosongkan variabel yang opsional.
- Nama usaha, warna, dan kontak diisi otomatis dari brand kit workspace kalau tidak dikirim. Kirim hanya kalau pengguna meminta yang berbeda.

## Kredit
Setiap video memotong kredit sebesar durasinya (misalnya 15 detik). Video yang gagal atau dibatalkan tidak dipotong. Kalau kredit tidak cukup (`insufficient_credits`), sarankan isi ulang di dasbor Mosio. Jangan membuat beberapa versi sekaligus tanpa diminta; tawarkan dulu.

## Kesalahan umum
- `invalid_variables`: lihat `details` untuk variabel yang salah, perbaiki, lalu coba lagi dengan `idempotency_key` baru.
- `too_many_active_renders`: tunggu video yang sedang diproses selesai.
- Video salah isi dan masih diproses: `cancel_video`, lalu buat ulang.
