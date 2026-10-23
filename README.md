# Mosio for Claude

Mosio turns a chat into a short motion video: promos, social content, explainers, ads and announcements as animated typography and motion graphics, vertical for Reels, TikTok and WhatsApp Status, or square, portrait and wide. Describe the video, approve the storyboard, check the moving preview in the chat, and get an MP4 back.

Claude directs: it writes the on-screen text and the motion as an HTML composition from one of Mosio's **recipes** (looks with a plan per beat), or fills in a **fill-in recipe** (a ready design where only the text changes). Mosio checks, previews and renders it on its servers. Mosio does not use AI models to generate images, video or audio: the motion is written as code.

## What's included

- The Mosio MCP server at `https://mcp.mosio.me/mcp`. You sign in with your Mosio account (OAuth).
- `video` (`/mosio:video`): the entry skill for any video: brand first, brief and storyboard, the video types (`skills/video/types/`), the choice between a fill-in recipe and a composition, and a review checklist.
- One skill per step, each callable on its own: `brief` (`/mosio:brief`), `storyboard`, `recipe` (the gallery and fill-in recipes), `brand`, `composition` (writing the video as an HTML composition), `motion` (craft notes, Hyperframes and GSAP libraries, more than 200 ready-made effects), and `review` (stills and findings before you see the preview).

The detailed rules, fonts, recipes with their example code, and the craft references are read from the Mosio server when Claude needs them (`get_composition_guide`, `list_recipes`, `skill_library`), so they are always current without a plugin update.

## Install

Claude.ai: add it from the plugin directory (**Customize > Plugins**), then connect Mosio on the plugin's **Connectors** tab and sign in.

Claude Code:

```
/plugin marketplace add usemosio/connector
/plugin install mosio@mosio
```

Then run `/mcp`, choose `mosio`, and sign in to Mosio. Start with `/mosio:video` or just ask for a video.

Without the plugin, the connector alone works too: Settings → Connectors → Add custom connector → `https://mcp.mosio.me/mcp`.

## Documentation

Guides and the REST API reference: https://docs.mosio.me

## Data sent to Mosio

- The plugin runs nothing on your computer and reads no local files. It has no hooks, scripts or local servers.
- Claude calls the Mosio server only for the tools you use. It sends the tool arguments: the brand id, a recipe id, on-screen text (for example a product name, a price, or a contact number you asked to show), the storyboard beats, the HTML composition Claude wrote, output size, and video ids. The rest of the conversation is not sent.
- Claude also reads guidance from the Mosio server: the composition rules, your brand kit, the recipe list and example code, and the craft library. These are read-only.
- Before showing you a preview, Claude can ask Mosio to inspect the composition: Mosio opens it on its servers and returns still frames and layout findings to Claude. Inspections are free and deleted after a day.
- Posting to social media (only when you ask): captions, planned times and drafts are stored in your Mosio workspace. When a post is published or scheduled, the video or the photos you uploaded, the caption and the chosen accounts go from Mosio to the platforms you connected (Instagram, Threads, TikTok) through Mosio's publishing partner. Accounts are connected and files uploaded in the Mosio dashboard, never in the chat.
- Sign-in uses OAuth. This repository holds no passwords or tokens.
- Videos are stored in your Mosio workspace. Download links expire after one hour.
- Previews and storyboards are free; rendering spends credits equal to the video's length. Claude never renders before you have seen the preview and asked for it.
- Privacy policy: https://mosio.me/en/privacy/
- Terms of service: https://mosio.me/en/terms/

## Credits

Parts of the `video`, `brief`, `storyboard` and `review` skills are adapted from these open-source skills: condensed and changed to fit Mosio's composition rules. Each file lists its sources and changes under "Adapted from", and `skills/video/NOTICE.md` carries the original licences.

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills)
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub)
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors)
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors)
- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana)
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

Plugin ini menghubungkan Claude dengan Mosio untuk membuat video motion pendek bagi usaha kecil dan kreator: promo, konten, edukasi, iklan, dan pengumuman (vertikal, kotak, portrait, atau lebar) dari recipe Mosio. Pasang dari direktori plugin Claude (atau connector `https://mcp.mosio.me/mcp`), masuk dengan akun Mosio, lalu minta videonya lewat chat. Data yang dikirim hanya argumen tool seperti di atas. Kebijakan privasi: https://mosio.me/en/privacy/ · Syarat layanan: https://mosio.me/en/terms/
