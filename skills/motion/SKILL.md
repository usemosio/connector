---
name: motion
description: "Motion craft and ready-made effects for a Mosio composition, read from the server: Mosio's craft notes (motion, transitions, kinetic type, captions, camera, numbers, logo, sizes, sound effects, and for the user's own footage: cuts, reframing, zooms, subtitles, glow and glass text), the official Hyperframes skills, GSAP, kinetic typography, short-form and video editing libraries, and the Hyperframes registry of more than 200 components (text effects, transitions, overlays, UI cards, charts). Use when the user asks for Hyperframes or GSAP, a specific effect or livelier motion (efek teks, transisi, animasi lebih hidup, glow, efek keren). Cutting the user's own footage starts in the edit skill, which reads the footage craft from here."
license: Proprietary
metadata:
  brand: Mosio
---

# Motion library

Craft for the `composition` skill. Everything here is read from the Mosio server with `skill_library` (index, read, search), so it is current. Mosio's own rules (`get_composition_guide`) win over anything a library says.

Always call `skill_library` yourself (no path returns the index) before writing motion: never tell the user a library is missing unless that call said so.

1. **Mosio craft first:** `skill_library` `mosio/video-composition/craft/`: `motion.md` (easing, rhythm, stagger), `transitions.md`, `kinetic-type.md`, `captions.md` (word by word), `camera.md` (one 2D world), `numbers.md` (counters, charts), `logo.md` (end card), `sizes.md` (one brief, several sizes), `sound.md` (which sound effect for which motion, and when; the catalog is `mosio/video-composition/sfx.md`); for the user's footage `editing.md` (cuts, trims, rhythm, sound), `reframe.md` (landscape in vertical), `zooms.md` (punch-ins, pushes), `subtitles.md` (speech captions); and `text-effects.md` (glow, glass, light sweeps, text behind things). These already follow Mosio's rules.
2. **Ready-made effects:** `search_registry` with English keywords ("kinetic text", "wipe transition", "price tag", "bar chart"), then `get_registry_item` and paste its HTML, CSS and JS into the composition, renamed to fit. A block is a whole scene: load it as a sub-composition at `compositions/<name>.html` (most are 1920×1080, so in a landscape video). GSAP and Google Fonts URLs inside are localised by Mosio.
3. **Third-party libraries:** the `skill_library` index lists them with a note each: official Hyperframes (`hyperframes/hyperframes-core/`, `hyperframes-animation/`, `hyperframes-creative/` and more), GSAP, kinetic typography, motion design, short-form, copywriting, film and creator styles, and video editing and captions (vertical editing, cut and zoom rules, caption craft, shot cards). Follow the note: on Mosio there is no CLI to run, transcripts only through `inspect_media`, no internet media as composition footage (only the user's own uploads from `list_media`), no CDN, and no AI image or video generation (a voice-over only through `create_voiceover`, with the workspace's own key).
4. Then back to the `composition` skill: write it, `inspect_composition`, `preview_composition`.

The motion serves the message, so each rule protects the reading of it: the event lands on the beat's key word (the eye goes where things move, so movement on another word pulls attention off the point); every beat moves into the next (a plain appear and disappear reads as slides, not a video); text to read holds at least 1.2 s (the time a short line needs on a phone); and the ending holds at least 2 s (so the contact can be read or screenshotted).
