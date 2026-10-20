# Mosio untuk Claude

Plugin ini membantu Claude membuat video promosi vertikal (Reels, TikTok, status WhatsApp) untuk usaha kecil dengan Mosio. Isinya: connector MCP Mosio di `https://mcp.mosio.me/mcp`, skill `buat-video-promo` (cara memilih template dan menulis teksnya), dan skill `sutradara-video` (cara Claude merancang sendiri videonya sebagai komposisi HTML, dengan contoh per gaya). Videonya dirender dari HTML (template Mosio atau komposisi yang ditulis Claude) di server Mosio, tanpa model AI yang membuat gambar, video, atau suara.

## Pasang di Claude Code

```
/plugin marketplace add usemosio/connector
/plugin install mosio@mosio
```

Lalu buka `/mcp`, pilih `mosio`, dan masuk dengan akun Mosio untuk memberi izin.

## Claude.ai

Tambahkan connector: Settings → Connectors → Add custom connector → `https://mcp.mosio.me/mcp`.
Skill di `skills/buat-video-promo/` dan `skills/sutradara-video/` bisa diunggah terpisah sebagai skill.

## Data yang dikirim

- Plugin ini tidak menjalankan program apa pun di komputer kamu dan tidak membaca file lokal.
- Claude memanggil connector di `https://mcp.mosio.me/mcp` hanya untuk tool yang kamu pakai. Yang dikirim adalah argumen tool: template, teks video (misalnya nama produk, harga, nomor kontak yang kamu minta tampil), komposisi HTML yang ditulis Claude, ukuran, dan id video. Isi percakapan lainnya tidak dikirim.
- Masuk lewat OAuth ke akun Mosio; plugin tidak menyimpan kata sandi atau token di repo ini.
- Video disimpan di akun Mosio kamu dan diunduh lewat link yang berlaku satu jam.
- Kebijakan privasi: https://app.mosio.me/privasi

## In English

This plugin lets Claude make vertical promotional videos for small businesses from Mosio templates. It adds the Mosio MCP connector (`https://mcp.mosio.me/mcp`, OAuth sign-in), a skill that explains how to pick a template and write the on-screen text, and a director skill for writing a video as an HTML composition. Videos are rendered from HTML (Mosio templates or compositions Claude writes) on Mosio's servers; no AI model generates images, video, or audio. The plugin runs nothing locally. Only tool arguments (template id, on-screen text, HTML composition, size, video id) are sent to the connector. Privacy policy: https://app.mosio.me/privasi
