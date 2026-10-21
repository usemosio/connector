# Mosio for Claude

Mosio renders short vertical videos (Reels, TikTok, WhatsApp Status) for small businesses. This plugin connects Claude to Mosio: ask for a promo video in the chat, check the preview, and get an MP4 back.

## What's included

- The Mosio MCP server at `https://mcp.mosio.me/mcp`. You sign in with your Mosio account (OAuth).
- `buat-video-promo`: how to pick a Mosio template and write the on-screen text.
- `sutradara-video`: how to write the video as an HTML composition, with 9 example styles in `skills/sutradara-video/gaya/`.
- `brief`: Brand intake, a short creative brief, and a second-by-second storyboard, all in chat, before any composition HTML is written. Use at the start of every new Mosio video, when the request is vague or missing facts (price, contact, size, duration), or when the user wants to plan or revise scenes (bikin video, brief, storyboard, konsep video, rencana adegan, naskah video, ubah adegan).
- `iklan`: Structure for paid performance video ads on Meta (Facebook, Instagram) and TikTok: a 0-3 s hook, context, payoff, proof, and a CTA held at least 2 s, with the hook message-matched to the CTA and the landing (WhatsApp chat, marketplace listing), sound-off readability, and A/B variants that change one variable. Use when the user wants an ad to run with budget (iklan, iklan FB, iklan IG, iklan TikTok, Meta Ads, boost postingan, konten iklan, A/B test, varian iklan, hook iklan).
- `promo`: Short 6-15 s sale spot that announces one offer: a big discount, a was-to-now strike-through price in Rupiah, a promo code, a deadline stated as text, and a held CTA, plus one template reused for many products. Use for discounts, flash sales, payday or twin-date campaigns (promo, diskon, flash sale, harga coret, potongan harga, kode promo, voucher, 11.11, 12.12, promo gajian, cuci gudang).
- `gerak`: Use when choreographing motion for a Mosio composition: picking moves (snap, squash and stretch, smear, overshoot, anticipation, iris or mask wipe, morph, follow-through), setting durations, eases and staggers, keeping direction continuous across cuts, and structuring the GSAP code so every frame is a pure function of timeline time (gerak, animasi, timing, easing, transisi terasa kaku, gerakan murahan). Pair with get_composition_guide.
- `review`: Use to critique a Mosio composition before calling preview_composition and again after seeing the preview or hearing the user's feedback: a never-ship list of AI-slop patterns, what good looks like, truth rules, the business-purpose-on-mute test, lazy defaults to question, and a pass/fail checklist to score every round (review, kritik, cek kualitas, kenapa videonya terasa murahan, sebelum render). Pair with get_composition_guide.

9 more director skills (`angka`, `chat`, `format`, `kinetic`, `logo`, `peluncuran`, `short-form`, `slideshow-foto`, `transisi`) are served by the MCP server's `skill_library` tool under `mosio/`.

Videos are rendered from HTML on Mosio's servers. Mosio does not use AI models to generate images, video, or audio.

## Install

Claude Code:

```
/plugin marketplace add usemosio/connector
/plugin install mosio@mosio
```

Then run `/mcp`, choose `mosio`, and sign in to Mosio.

Claude.ai: Settings → Connectors → Add custom connector → `https://mcp.mosio.me/mcp`. Each skill under `skills/` can also be uploaded as a skill on its own.

## Documentation

Guides and the REST API reference: https://docs.mosio.me

## Data sent to Mosio

- The plugin runs nothing on your computer and reads no local files.
- Claude calls the Mosio server only for the tools you use. It sends the tool arguments: template id, on-screen text (for example a product name, a price, or a contact number you asked to show), the HTML composition Claude wrote, output size, and video id. The rest of the conversation is not sent.
- Sign-in uses OAuth. This repository holds no passwords or tokens.
- Videos are stored in your Mosio workspace. Download links expire after one hour.
- Privacy policy: https://mosio.me/en/privacy/
- Terms of service: https://mosio.me/en/terms/

## Credits

The director skills (`brief`, `iklan`, `promo`, `gerak`, `review` and the ones in `skill_library`) are adapted from these open-source skills: translated to Indonesian, condensed, and changed to fit Mosio's composition rules. Each skill lists its sources and changes under "Diadaptasi dari", carries a `NOTICE.md` with the original licences, and all of them are in `THIRD_PARTY_NOTICES.md`.

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills)
- [iart-ai/data-animation-skills](https://github.com/iart-ai/data-animation-skills) (MIT, © iart.ai)
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen dan kontributor HyperFrames)
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub)
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors)
- [iart-ai/text-message-video-skills](https://github.com/iart-ai/text-message-video-skills) (MIT, © iart.ai)
- [smwbev/framewright](https://github.com/smwbev/framewright) (MIT, © Evgenii (smwbev))
- [lovisdotio/motion2code](https://github.com/lovisdotio/motion2code) (MIT, © Lovis Odin)
- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana)
- [LottieFiles/motion-design-skill](https://github.com/LottieFiles/motion-design-skill) (MIT, © LottieFiles)
- [iart-ai/ad-video-skills](https://github.com/iart-ai/ad-video-skills) (MIT, © iart.ai)
- [iart-ai/kinetic-typography-skills](https://github.com/iart-ai/kinetic-typography-skills) (MIT, © iart.ai)
- [iart-ai/motion-design-skills](https://github.com/iart-ai/motion-design-skills) (MIT, © iart.ai)
- [AbubakrChan/product-launch-motion](https://github.com/AbubakrChan/product-launch-motion) (MIT, © Abubakr Chan)
- [iart-ai/ecommerce-video-skills](https://github.com/iart-ai/ecommerce-video-skills) (MIT, © iart.ai)
- [echris6/motion-video-kit](https://github.com/echris6/motion-video-kit) (MIT, © echris6)
- [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) (MIT, © Kyle Zantos)
- [howseen-ai/claude-motion-design](https://github.com/howseen-ai/claude-motion-design) (MIT, © Howseen AI (Raphaël Aubry))
- [crawfordxx/xiaoma-studio](https://github.com/crawfordxx/xiaoma-studio) (MIT, © xiaoma-studio contributors)

"HyperFrames", "HeyGen" and "GSAP" are trademarks of their owners and are named here only as sources.

## Bahasa Indonesia

Plugin ini menghubungkan Claude dengan Mosio untuk membuat video promosi bagi usaha kecil (vertikal, kotak, portrait, atau lebar). Pasang connector `https://mcp.mosio.me/mcp`, masuk dengan akun Mosio, lalu minta videonya lewat chat. Data yang dikirim hanya argumen tool seperti di atas. Kebijakan privasi: https://mosio.me/en/privacy/ · Syarat layanan: https://mosio.me/en/terms/
