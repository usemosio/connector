# Mosio for Claude

Mosio renders short videos for small businesses and creators: promos, social content, explainers, ads and announcements, vertical (Reels, TikTok, WhatsApp Status) or square, portrait and wide. This plugin connects Claude to Mosio: describe the video in the chat, approve the storyboard, check the preview, and get an MP4 back.

## What's included

- The Mosio MCP server at `https://mcp.mosio.me/mcp`. You sign in with your Mosio account (OAuth).
- `video`: the entry skill for any video: brief and storyboard, the choice between a template and a custom composition, a review checklist, and one guide per video type (`skills/video/types/`).
- `video-template`: how to pick a Mosio template and write its on-screen text.
- `video-composition`: how to write the video as an HTML composition, with 9 example styles in `skills/video-composition/styles/` and craft references in `skills/video-composition/craft/`.

The same skills are served to every assistant by the MCP server's `skill_library` tool under `mosio/`.

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

Parts of the `video` and `video-composition` skills are adapted from these open-source skills: condensed and changed to fit Mosio's composition rules. Each file lists its sources and changes under "Adapted from", each skill carries a `NOTICE.md` with the original licences, and all of them are in `THIRD_PARTY_NOTICES.md`.

- [iart-ai/kinetic-typography-skills](https://github.com/iart-ai/kinetic-typography-skills) (MIT, © iart.ai)
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors)
- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana)
- [iart-ai/motion-design-skills](https://github.com/iart-ai/motion-design-skills) (MIT, © iart.ai)
- [lovisdotio/motion2code](https://github.com/lovisdotio/motion2code) (MIT, © Lovis Odin)
- [LottieFiles/motion-design-skill](https://github.com/LottieFiles/motion-design-skill) (MIT, © LottieFiles)
- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills)
- [iart-ai/data-animation-skills](https://github.com/iart-ai/data-animation-skills) (MIT, © iart.ai)
- [smwbev/framewright](https://github.com/smwbev/framewright) (MIT, © Evgenii (smwbev))
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub)
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors)
- [echris6/motion-video-kit](https://github.com/echris6/motion-video-kit) (MIT, © echris6)
- [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) (MIT, © Kyle Zantos)
- [howseen-ai/claude-motion-design](https://github.com/howseen-ai/claude-motion-design) (MIT, © Howseen AI (Raphaël Aubry))
- [iart-ai/ad-video-skills](https://github.com/iart-ai/ad-video-skills) (MIT, © iart.ai)
- [AbubakrChan/product-launch-motion](https://github.com/AbubakrChan/product-launch-motion) (MIT, © Abubakr Chan)
- [iart-ai/ecommerce-video-skills](https://github.com/iart-ai/ecommerce-video-skills) (MIT, © iart.ai)
- [iart-ai/text-message-video-skills](https://github.com/iart-ai/text-message-video-skills) (MIT, © iart.ai)
- [crawfordxx/xiaoma-studio](https://github.com/crawfordxx/xiaoma-studio) (MIT, © xiaoma-studio contributors)

"HyperFrames", "HeyGen" and "GSAP" are trademarks of their owners and are named here only as sources.

## Bahasa Indonesia

Plugin ini menghubungkan Claude dengan Mosio untuk membuat video pendek bagi usaha kecil dan kreator: promo, konten, edukasi, iklan, dan pengumuman (vertikal, kotak, portrait, atau lebar). Pasang connector `https://mcp.mosio.me/mcp`, masuk dengan akun Mosio, lalu minta videonya lewat chat. Data yang dikirim hanya argumen tool seperti di atas. Kebijakan privasi: https://mosio.me/en/privacy/ · Syarat layanan: https://mosio.me/en/terms/
