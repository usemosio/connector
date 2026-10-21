---
name: promo
description: "Short 6-15 s sale spot that announces one offer: a big discount, a was-to-now strike-through price in Rupiah, a promo code, a deadline stated as text, and a held CTA, plus one template reused for many products. Use for discounts, flash sales, payday or twin-date campaigns (promo, diskon, flash sale, harga coret, potongan harga, kode promo, voucher, 11.11, 12.12, promo gajian, cuci gudang)."
license: "MIT; lihat bagian Diadaptasi dari"
metadata:
  author: Mosio
  version: "0.1.0"
  sources: "ecommerce-video-skills@2bd0dd1"
---

# Video promo dan harga coret

Spot 6-15 detik yang mengumumkan **satu penawaran** dan membuat orang bertindak sekarang: diskon mendarat, harga lama dicoret, harga baru turun, kode dan batas waktu terbaca, ajakan menutup. Bukan iklan fitur (pakai `iklan`) dan bukan testimoni. Jalankan `brief` dulu; aturan teknis dari `get_composition_guide`.

Gaya dasar: `flash-sale` (persen meledak, pita berjalan). Untuk satu produk dengan harga besar, `product-hero`. Kedua gaya belum punya harga coret dan chip kode: tambahkan dari resep di bawah.

## Yang ditanyakan sebelum mulai

Semua dari pengguna, tidak ada yang ditebak:
- Harga awal dan harga promo (atau harga awal dan persen diskon), dalam Rupiah.
- Kode promo, kalau ada, ditulis persis.
- Batas waktu: tanggal dan jam, atau syarat yang benar ("selama stok ada" hanya kalau memang begitu).
- Produk atau kategori yang didiskon, dan di mana belinya (WA, Shopee, Tokopedia, toko fisik).

Kalau persen dan dua harga tidak cocok (Rp100.000 ke Rp70.000 disebut "diskon 50%"), tanyakan mana yang benar sebelum menulis apa pun.

## Urutan beat

| Beat | Tugas | 6 detik | 10 detik | 15 detik |
| --- | --- | --- | --- | --- |
| Hook | momen dan nama toko menghentak ("PROMO GAJIAN") | 0-1 | 0-1,5 | 0-1,5 |
| Diskon | angka besar mendarat ("40%") | 1-2,5 | 1,5-4 | 1,5-4,5 |
| Harga coret | harga lama dicoret, harga baru turun | 2,5-4 | 4-7 | 4,5-8,5 |
| Produk | satu visual produk | (dibuang) | di bawah harga, 4-7 | 8,5-11 |
| Kode dan batas waktu | chip kode dan tenggat, diam | (digabung ke ajakan) | 7-8 | 11-12,5 |
| Ajakan | "Belanja sekarang", toko, kontak, diam ≥2 detik | 4-6 | 8-10 | 12,5-15 |

- Satu beat, satu tugas. Jangan menumpuk dua.
- Hook tidak pernah dipanjangkan. Yang memanjang di versi 15 detik adalah napas tiap beat.
- Angka diskon adalah elemen paling besar di layar: lebih besar dari nama toko dan produk. Archivo Black atau Bebas Neue cocok.

## Dua aturan mati

1. **Angka adalah pesannya, jadi harus tepat.** Harga promo atau persen dihitung dari data, tidak diketik dua kali. Video promo dengan harga salah lebih buruk daripada tidak ada video.
2. **Satu penawaran per video.** Satu diskon, satu kode, satu ajakan. Ada dua promo? Buat dua video.

Pilih bingkai sesuai harga: barang mahal pakai persen ("40%" lebih kuat daripada "Hemat Rp52.000"), barang murah pakai nominal ("Hemat Rp5.000" lebih kuat daripada "22%").

## Resep harga coret

Urutannya penting: coret dulu, baru harga baru turun. Coret dan hitung di detik yang sama terlihat seperti glitch.

```html
<div id="persen" class="persen">31%</div>
<div class="harga">
  <span class="lama"><span id="awal">Rp129.000</span><i id="coret"></i></span>
  <div class="topeng"><div id="promo">Rp129.000</div></div>
</div>
```
```css
.lama { position: relative; opacity: .55; }
#coret { position: absolute; left: -4%; right: -4%; top: 52%; height: 8px; background: var(--aksen); transform-origin: left center; }
.topeng { overflow: hidden; }
#promo { font-variant-numeric: tabular-nums; }
```
```js
const HARGA = { awal: 129000, promo: 89000 };          // dari pengguna, satu-satunya tempat angka diketik
const persen = Math.round((1 - HARGA.promo / HARGA.awal) * 100);
const rp = (n) => "Rp" + n.toLocaleString("id-ID");
document.getElementById("awal").textContent = rp(HARGA.awal);
document.getElementById("persen").textContent = persen + "%";
const elPromo = document.getElementById("promo");
const o = { v: HARGA.awal };
tl.fromTo("#coret", { scaleX: 0 }, { scaleX: 1, duration: 0.4, ease: "power2.out" }, 4.0)
  .fromTo("#promo", { yPercent: 110 }, { yPercent: 0, duration: 0.5, ease: "expo.out" }, 4.3)
  .to(o, { v: HARGA.promo, duration: 0.9, ease: "power3.out", onUpdate: () => {
      // Bulat ke ribuan saat berjalan supaya tidak berkedip, angka persis di akhir.
      elPromo.textContent = rp(o.v === HARGA.promo ? o.v : Math.round(o.v / 1000) * 1000);
    } }, 4.3)
  .to("#promo", { scale: 1.1, duration: 0.15, yoyo: true, repeat: 1, ease: "power2.out" }, 5.2);
```

- `tl` adalah timeline utama yang di-pause dan didaftarkan di `window.__timelines["main"]`.
- Garis coret memakai `scaleX`, bukan `width`. `transform-origin` di CSS boleh; yang tidak boleh adalah `transform` awal di CSS.
- Harga baru diam setidaknya 1,2 detik setelah mendarat.
- Format Rupiah: `Rp89.000` (titik ribuan, tanpa spasi, tanpa ",00"). Pakai `tabular-nums` supaya digit tidak goyang.

## Kode promo

- Tampilkan sebagai chip: garis putus-putus warna aksen, latar sedikit lebih terang, huruf tebal dengan jarak huruf lebar (Space Grotesk 700, atau huruf mono dari Google Fonts).
- Kode diam saat terbaca, minimal 2 detik. Kode yang bergerak tidak bisa dibaca atau di-screenshot.
- Tulis persis dari pengguna. Kode yang memuat urgensi biasanya lebih kuat (contoh ilustratif: `GAJIAN25`, `KILAT40`, `HARIINI`), tapi jangan mengganti kode asli.

## Batas waktu: teks, bukan jam

Video ditonton kapan saja: besok, minggu depan, setelah promo selesai. Karena itu batas waktu ditulis sebagai teks yang tetap benar kapan pun ditonton:
- "Berakhir Minggu, 23.59 WIB", "Hanya 12.12", "Sampai 31 Desember", "Hari ini saja" (kalau videonya memang untuk hari itu).
- Tanpa jam yang berjalan dari waktu nyata. `Date.now()` dan `new Date()` juga dilarang guide.
- Gaya `flash-sale` punya hitung mundur jam:menit:detik yang turun cepat. Itu dekorasi yang bisa menyesatkan; ganti dengan teks tenggat di atas. Kalau ingin ritme hitung mundur, pakai hitungan masuk "3, 2, 1, MULAI" di hook, bukan sisa waktu promo.
- Urgensi harus nyata. "Stok tinggal 5" hanya kalau pengguna mengatakannya.
- Tahan beat tenggat setidaknya 2 detik; jangan dikedipkan.

## Bahasa gerak: bertenaga, tidak kacau

- Satu kurva masuk untuk semua beat (misalnya `back.out(1.6)` atau `expo.out`), supaya cepatnya terasa yakin.
- Satu warna aksen untuk hemat (merah, kuning, atau hijau terang) dipakai di persen, garis coret, dan tombol.
- Ganti beat dengan potongan tegas atau wipe cepat. Hindari gerak melayang dan parallax yang melunakkan urgensi; latar cukup hidup pelan (grid bergeser, pita berjalan dengan `repeat` angka tertentu).
- Penutup: nama toko, ajakan, kontak dari brand kit, diam ≥2 detik, keluar di 0,5 detik terakhir.

## Produk

Gambar hanya `data:` kecil (batas komposisi 200.000 karakter). Tanpa foto, cukup nama produk besar dengan bentuk SVG sederhana atau ikon kategori. Jangan pakai foto dari internet.

## Satu template, banyak produk

Promo sering untuk banyak barang. Buat satu komposisi, lalu per produk ganti datanya saja: angka di `HARGA`, teks di HTML yang ditandai. Tata letak, warna, dan waktu tetap.

```html
<!-- GANTI per produk: nama, kode, tenggat. Harga hanya di const HARGA. -->
<div class="nama">Sneakers Langkah Run</div>
<div class="kode">KILAT35</div>
<div class="tenggat">Berakhir Minggu, 23.59 WIB</div>
```

- Cek sebelum pratinjau: `HARGA.promo < HARGA.awal`, kode tidak kosong kalau dipakai, tenggat ada.
- Cek panjang: uji nama produk terpanjang dan harga dengan digit terbanyak (Rp1.250.000) supaya tidak terpotong di zona aman.
- Pratinjau satu produk dulu sampai beres, baru produk lain. Setiap render memotong kredit sebesar durasi: tawarkan jumlahnya sebelum membuat semuanya.
- Judul per video menyebut produknya ("Promo Gajian: Sneakers Langkah Run").

## Cek sebelum dikirim

- Persen dan harga dihitung dari satu sumber, tepat dalam Rupiah.
- Coret dulu, harga baru sesudahnya; keduanya di timeline.
- Satu penawaran, satu kode, satu ajakan; angka diskon elemen terbesar.
- Tenggat berupa teks yang benar kapan pun video ditonton, ditahan ≥2 detik.
- Semua harga, kode, tenggat, dan kontak dari pengguna atau brand kit.

Lebih lanjut di `skill_library`: angka berganti `short-form/countdown-video/references/digit-flip.md`, angka naik `hyperframes/hyperframes-animation/blueprints/dataviz-countup.md`, pita teks `hyperframes/hyperframes-animation/blueprints/ticker-takeover.md`, tombol ajakan `hyperframes/hyperframes-animation/blueprints/cta-morph-press.md`.

## Diadaptasi dari

- [iart-ai/ecommerce-video-skills](https://github.com/iart-ai/ecommerce-video-skills) (MIT, © iart.ai), commit `2bd0dd1`: `skills/promo-video/SKILL.md`, `skills/promo-video/references/promo-components.md`.

Perubahan oleh Mosio: diterjemahkan dan diringkas ke bahasa Indonesia, kode Remotion diganti resep GSAP di timeline yang di-pause (tanpa audio, tanpa CDN), dan format harga diganti Rupiah. Hitung mundur dari jam nyata dibuang dan diganti tenggat berupa teks; skrip batch dan validasi file diganti blok data per produk dan pratinjau; contoh diganti konteks UMKM dan kampanye Indonesia. Teks lisensi lengkap: `skills/licenses/` di repo Mosio, atau NOTICE.md di samping skill ini pada plugin.
