# Mosio for Claude

Mosio turns a chat into a short motion video: promos, social content, explainers, ads and announcements as animated typography and motion graphics, vertical for Reels, TikTok and WhatsApp Status, or square, portrait and wide. Describe the video, approve the storyboard, check the moving preview in the chat, and get an MP4 back.

Claude directs: it writes the on-screen text and the motion as an HTML composition from one of Mosio's **recipes** (looks with a plan per beat). Mosio checks, previews and renders it on its servers. Neither this plugin nor the connector uses AI models to generate images or video: the motion is written as code. When you ask for a voice-over, it is spoken by Google Gemini or ElevenLabs with your workspace's own key.

## What's included

- The Mosio MCP server at `https://mcp.mosio.me/mcp`. You sign in with your Mosio account (OAuth).
- `video` (`/mosio:video`): the entry skill for any video: the goal first (sell, launch, a story, a lesson), then the brand when there is one, the brief, the concept and the storyboard, the video types (`skills/video/types/`), the recipe, and a review checklist.
- One skill per step, each callable on its own: `brief` (`/mosio:brief`), `storyboard`, `recipe` (the gallery and picking a recipe), `brand`, `composition` (writing the video as an HTML composition), `motion` (craft notes, Hyperframes and GSAP libraries, more than 200 ready-made effects), `edit` (editing your own footage: cuts, reframing, zooms, subtitles), and `review` (stills and findings before you see the preview).

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

- The plugin itself runs nothing on your computer and reads no local files. It has no hooks, scripts or local servers.
- Claude calls the Mosio server only for the tools you use. It sends the tool arguments: the brand id, a recipe id, on-screen text (for example a product name, a price, or a contact number you asked to show), the storyboard beats, the HTML composition Claude wrote, output size, and video and media ids. The rest of the conversation is not sent.
- Claude also reads from the Mosio server: the composition rules, your brand kit, the recipe list and example code, the craft library, and the list of photos, footage and sound in your workspace's media (file name, kind, dimensions and length). These are read-only.
- Topic research sends your question and optional context to Anthropic and its web search service, then returns cited findings for review.
- Saved production plans contain your brief, cited research supplied by the assistant, storyboard and audio cues. You can resume or delete them through the connector; saving is not approval to render or publish. Public social reference links can be imported for analysis; recipe creation sends the video to Meta in contributor mode (which permits training) and its analysis to Anthropic only with your explicit consent.
- When you ask, Claude adds a brand or changes one (name, voice, colours, fonts, contact, products and prices) in your Mosio workspace.
- Your own photos, footage and sound (only the files you choose): in the upload card in the chat you pick files on your device, and each goes straight from your device to your Mosio workspace's storage. In Claude Code, Claude can instead send a file from your computer with the upload command Mosio returns (a short-lived upload link), run like any shell command with your approval. Files can also be uploaded on the dashboard's Media page. Mosio prepares a copy of each file for video on its servers; the copy carries no metadata (location, device).
- When Claude looks at one of your files, Mosio returns its length, a sheet of numbered stills, scene cuts, silences, loudness, and an automatic transcript of any speech, made on Mosio's servers with whisper.cpp speech recognition. Files, stills and transcripts stay in your workspace until you delete the file.
- When you ask for text behind a person, Claude asks Mosio to cut that person out of a few seconds of one of your videos (`make_cutout`): Mosio runs an image segmentation model (MODNet) on its servers that separates the person from the background and writes a transparent clip of that stretch; it creates no new imagery. The cutout stays in your workspace until you delete the video.
- Voice-overs, music and sound effects (only when you ask, and only when an owner or admin of your workspace saved a Google Gemini or ElevenLabs key in the Mosio dashboard): Claude sends the script (or the dialogue lines), the chosen model and voice, or the music or sound-effect prompt to Mosio, which sends them with that key to the provider; the provider bills your own account. The audio is added to your workspace's media like an upload; ElevenLabs also returns each word's timing, kept with the file for captions. Keys are never entered in the chat, and voices are designed only in the dashboard. Gemini voice samples you play were made once by Mosio and are shared by every workspace.
- Before showing you a preview, Claude can ask Mosio to inspect the composition: Mosio opens it on its servers and returns still frames and layout findings to Claude. Inspections are free and deleted after a day.
- In Claude Code, when you ask to check or render a video on your own computer, Mosio returns the packed composition and the commands to run Mosio's command-line tool (`@usemosio/frames` on npm); Claude runs them only with your approval.
- Posting to social media (only when you ask): captions, planned times and drafts are stored in your Mosio workspace. When a post is published or scheduled, the video or the photos you uploaded, the caption and the chosen accounts go from Mosio to the platforms you connected (Instagram, Threads, TikTok) through Mosio's publishing partner. Accounts are connected in the Mosio dashboard, never in the chat.
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
- [browser-use/video-use](https://github.com/browser-use/video-use) (MIT, © Browser Use)
- [AgriciDaniel/claude-shorts](https://github.com/AgriciDaniel/claude-shorts) (MIT, © Daniel Agrici)
- [iart-ai/b-roll-skills](https://github.com/iart-ai/b-roll-skills) (MIT, © iart.ai)
- [kurbaitaev/ghost-editor](https://github.com/kurbaitaev/ghost-editor) (MIT, © kurbaitaev)
- [kmilo93sd/agent-video-editor](https://github.com/kmilo93sd/agent-video-editor) (MIT, © kmilo93sd)
- [crawfordxx/xiaoma-studio](https://github.com/crawfordxx/xiaoma-studio) (MIT, © xiaoma-studio contributors)

"HyperFrames", "HeyGen" and "GSAP" are trademarks of their owners and are named here only as sources.

## Bahasa Indonesia

Plugin ini menghubungkan Claude dengan Mosio untuk membuat video motion pendek bagi usaha kecil dan kreator: promo, konten, edukasi, iklan, dan pengumuman (vertikal, kotak, portrait, atau lebar) dari recipe Mosio. Pasang dari direktori plugin Claude (atau connector `https://mcp.mosio.me/mcp`), masuk dengan akun Mosio, lalu minta videonya lewat chat. Data yang dikirim hanya argumen tool seperti di atas, plus foto, video, dan suara yang kamu pilih sendiri untuk diunggah. Kebijakan privasi: https://mosio.me/en/privacy/ · Syarat layanan: https://mosio.me/en/terms/
