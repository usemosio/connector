# Mosio for Claude

Mosio renders short vertical videos (Reels, TikTok, WhatsApp Status) for small businesses. This plugin connects Claude to Mosio: ask for a promo video in the chat, check the preview, and get an MP4 back.

## What's included

- The Mosio MCP server at `https://mcp.mosio.me/mcp`. You sign in with your Mosio account (OAuth).
- `buat-video-promo`: how to pick a Mosio template and write the on-screen text.
- `sutradara-video`: how to write the video as an HTML composition, with six example styles in `skills/sutradara-video/gaya/`.

Videos are rendered from HTML on Mosio's servers. Mosio does not use AI models to generate images, video, or audio.

## Install

Claude Code:

```
/plugin marketplace add usemosio/connector
/plugin install mosio@mosio
```

Then run `/mcp`, choose `mosio`, and sign in to Mosio.

Claude.ai: Settings → Connectors → Add custom connector → `https://mcp.mosio.me/mcp`. The two skills under `skills/` can also be uploaded as skills on their own.

## Data sent to Mosio

- The plugin runs nothing on your computer and reads no local files.
- Claude calls the Mosio server only for the tools you use. It sends the tool arguments: template id, on-screen text (for example a product name, a price, or a contact number you asked to show), the HTML composition Claude wrote, output size, and video id. The rest of the conversation is not sent.
- Sign-in uses OAuth. This repository holds no passwords or tokens.
- Videos are stored in your Mosio workspace. Download links expire after one hour.
- Privacy policy: https://app.mosio.me/privasi

## Bahasa Indonesia

Plugin ini menghubungkan Claude dengan Mosio untuk membuat video promosi vertikal bagi usaha kecil. Pasang connector `https://mcp.mosio.me/mcp`, masuk dengan akun Mosio, lalu minta videonya lewat chat. Data yang dikirim hanya argumen tool seperti di atas. Kebijakan privasi: https://app.mosio.me/privasi
