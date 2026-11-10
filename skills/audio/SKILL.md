---
name: audio
description: "Add a voice-over, a dialogue, music or custom sound effects to a video with the workspace's own Gemini or ElevenLabs key, and the free bundled sound library: script sized to the video, voice picked from samples, delivery tags, takes replaced instead of piled up, word times for captions, music mixed under speech. Use for voiceover, narasi, dubbing suara, dialog dua orang, musik latar, efek suara, ganti suara."
license: Proprietary
metadata:
  brand: Mosio
---

# Voice-over, music and sound

This is one step of the `video` skill's flow (workspace, goal, brand, material (Notes, research, Discover, references), type, brief, look (the recipe), storyboard, build, review, preview, render, post). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

A voice-over is spoken by the user's own Gemini or ElevenLabs account and billed there, not in credits; a wrong take costs them money and clutters their media. So the script is approved before it is spoken.

1. **Script first.** Write it in the brand's voice and the video's language, sized to the video: about 12-15 characters a second, so a 20 s video holds about 240-300 characters. Longer and the speaker rushes or the video overruns. Write it as it is spoken: numbers and abbreviations spelled out ("dua puluh ribu", not "20rb"). Show it and wait for a yes.
2. **Voice.** `list_voices` shows a card where the user plays samples and picks one (the pick arrives as their next message); call it when they want to choose or describe a voice ("suara cewek yang ceria"). Gemini also has Indonesian and Javanese voices (`language`). No key: the result gives `settings_url`; send them there. Never ask for a key in the chat.
3. **Speak it.** `create_voiceover` with the script (and `project_id` when the video exists). Delivery, a few tags only, where a person would:
   - Gemini: `style` ("warm, upbeat, a little fast") and inline tags `<laugh>`, `<sigh>`, `<breath>`, `<short pause>`, `<long pause>`; CAPITALS stress a word.
   - ElevenLabs `eleven_v3`: `[excited]`, `[whispers]`, `[laughs]`, `[sighs]`.
   - A conversation (seller and customer, two hosts, a chat story): `lines`, one item per turn with its `speaker` and `voice`. Gemini speaks two speakers; ElevenLabs up to ten (2,000 characters in all).
4. **Another take:** call `create_voiceover` again with the earlier one as `replaces`, so takes do not pile up in the user's media.
5. **Sync.** Once `list_media` lists the file, play it with `<audio>` at its `path`. `inspect_media` with `words` gives each word's start and end (at once for ElevenLabs, a few minutes later for Gemini); land each beat's motion and caption on its spoken word. The recording sets the video's length.
6. **Music and sound effects.** Common whooshes, pops and clicks: the free bundled library (`get_composition_guide` sound kit, `skill_library` `mosio/video-composition/sfx.md`). Custom ones need an ElevenLabs key: `create_sound` with `kind: "music"` (an English prompt: genre, mood, instruments, tempo; instrumental; as long as the video) or `"sound_effect"` (one sound, up to 30 s). Keep music about -18 dB under speech, so every word stays clear on a phone speaker, and fade it out at the end.
7. Save the script and cues in the production plan (`save_production_plan`), then back to the `composition` skill and a preview before any render.

- Never clone or imitate a real person's voice the user has no right to use.
- Most viewers watch on mute: every spoken word still appears on screen.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
