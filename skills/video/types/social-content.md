# Social content: regular posts that get watched to the end

Everyday content for Reels, TikTok, Shorts and WhatsApp Status: tips, behind the scenes, before/after, a series of posts, trends. Usually 9:16 and 6-30 s. Use this when planning the storyboard (`../brief.md`) and when judging the preview (`../review.md`). Sizes, safe zones and technical rules come from `get_composition_guide`. In one line: every frame is a function of timeline time, and every second must earn the next.

## Seven ground rules

1. **Transform, don't cut.** Scenes change through a shape that morphs, a colour that floods, text revealed from behind a line, a zoom through an object. Hard cuts only on decisive moments (price lands, before/after).
2. **One background, one text colour, one accent.** One heading face and one body face (two families max).
3. **On-screen text is the narration.** Renders are silent and most people watch muted.
4. **Something moves in the first 2 s**: a glass drops in, a chat arrives, a button is pressed. Not a gradient with a centred title fading in.
5. **A new event every 2-4 s, but only one thing to read at a time.** Motion may be fast; meaning gets time.
6. **The end echoes the start.** The last frame reuses the opening object, line or shape in its changed state.
7. **Facts need a source.** Numbers, ratings, customer counts and claims only from the user. No number? No number.

## Pick the format

| Format            | Shape                                                                                     | Watch for                                                                                                               |
| ----------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Tips              | hook "3 cara ..." then one tip per screen, numbered ("1/3"), then recap or CTA            | 3 tips is strongest; each tip 7 words max                                                                               |
| Behind the scenes | process steps in order (dough, oven, packing), one step per scene                         | no photos? SVG shapes labelled "Ilustrasi"                                                                              |
| Before/after      | same framing both sides, a wipe or hard cut on the turn, then the result held             | only the user's own photos; never fake a result                                                                         |
| Series            | same opening frame, colours and layout every episode; "Bagian 2/5" chip                   | one composition, swap only text per episode; each episode stands alone; end with "Besok: ..." only if the user plans it |
| Trend             | a text-led trend shape ("POV: ...", "Hal yang saya tahu setelah ...") in the brand's look | trend audio is added in the app after render; never promise motion synced to a song                                     |
| WhatsApp Status   | contacts who already know the shop; one tap skips it                                      | hook works in 1 s; direct CTA "Balas status ini untuk pesan"; keep it short                                             |

One video, one idea. If the point does not fit in one sentence, it is not ready. A second idea is a second video.

## Structure and time budget

| Part   | Job                                                 | 15 s video      |
| ------ | --------------------------------------------------- | --------------- |
| Hook   | open one question or promise, moving from frame one | 0-1.5           |
| Body   | 2-4 scenes, one idea each, the question stays open  | 1.5-12          |
| Answer | close the hook's question: product, price, result   | last body scene |
| Ending | CTA, business name, contact, held 2 s or more       | 12-15           |

- Usual roles: hook → fact → fact → payoff → cta. Tips use one `step` per tip; before/after uses `compare`.
- A 30 s video grows the body. The hook stays about 1.5 s.
- Never open with a lone logo, a greeting ("Halo kak"), or black for more than 0.3 s.

## Read points

Viewers watch once, at full speed. You know the video; they don't. For each scene list what they must catch, in order, with times:

```
Scene 2 (1.5-4.5)
  1.5-2.3  glass rises to centre (eye goes to centre)
  2.3-3.6  "Es Kopi Susu Gula Aren" reads
  3.6-4.5  accent on "Gula Aren", then transition
```

- One read point at a time. Split cause and effect; never stack them.
- Read time = find + understand + pause. Small, far, fast or faint things need longer. Text to be read holds still at least 1.2 s.
- Guide the eye first: nudge or light the spot just before an important read.
- Scene length follows its read points. Leave at least 0.8 s after the last one before the transition.

## Pacing

- New event every 2-4 s, at uneven gaps (2.4, then 3.1, then 2.6). Metronome rhythm bores.
- Rhythm comes from the timeline, not a song.
- The background is always slightly alive (1-3% slow zoom, drifting gradient or grid); text being read stays still.
- No meaningless pause longer than 1 s.

## Text density and look

- 7 words on screen max: one heading plus at most two short support lines, sizes from the guide. Lines that carry information (strikes, charts) at least 2 px.
- No full stop at the end of on-screen lines; ? and ! are fine. Small labels in caps, tracked 0.2-0.4 em. Numbers in `tabular-nums`.
- Accent only on the most important thing in that second, about 10% of the screen or less. A second accent only for the opponent: old price, "sebelum", a mistake.
- Dark backgrounds are tinted black (`#0b0a08`, `#0b080b`), light ones paper (`#f4f2ee`, `#ece4d4`); derive from the brand with `color-mix()`. Flipping dark to light at the turn (problem to solution) is a cheap, strong rhythm tool.
- Glow only on the hero (product, number). Vignette with `radial-gradient`; no full-screen blur or `backdrop-filter`.

One look per video; borrow another for one scene at most:

| Content                           | Look                                             | Nearest Mosio style              |
| --------------------------------- | ------------------------------------------------ | -------------------------------- |
| Slogan, announcement, tips        | alternating full colour, big bold type           | `kinetic-type`, `flash-sale`     |
| One product and price             | warm dark ground, glowing product centred        | `product-hero`                   |
| App, SaaS, digital service        | warm white canvas, black UI, one accent          | `peluncuran-aplikasi`            |
| Facts, milestones, comparisons    | black, white, grey plus one colour, left-aligned | `angka-capaian`, `dasbor-iklan`  |
| Home bakery, crafts, warm stories | paper colours, flat layers, 3-5 fixed colours    | `menu-board` as a starting point |

Transitions (colour flood, shape morph, line reveal, zoom through, iris, match cut, cut on motion): see `../../video-composition/craft/transitions.md`. Avoid crossfade as default, blur-ins and 3D flips.

## Endings and loops

- The ending holds 2 s or more (guide rule). Echo the opener: the glass from second 0 stands beside "Pesan via WA"; the hook's question is answered with its own words.
- For a loop with no CTA (showcase content), make the last frame match the first: same position, colour, composition. Loops work best on short videos.

## Banned

Gradient-plus-centred-title openers; every element with the same fade; generic particles or bursts; shockwave rings; RGB split; camera shake; lens flares; neon glow (unless it is the theme); grid floors; flashing backgrounds; bouncy easing on UI; tech labels in all four corners; lines ending in a full stop; two important things appearing at once.

## Judge the preview

After `preview_composition`, score 1-10: first-2-s hook, readable on a phone, motion quality, event density, look consistency. Fix the worst three and preview again until all are 8 or more. Then check: something moves at 0-2 s; one event per scene with no stacked read points; every scene change has a transition; the ending echoes the opener and holds 2 s.

More (hook patterns, pattern interrupts, platform safe zones): `skill_library` `short-form/short-form-video/SKILL.md` and `short-form/short-form-video/references/retention-pacing.md`. Their examples are Remotion; translate them to tweens on the Mosio timeline.

## Adapted from

- [crawfordxx/xiaoma-studio](https://github.com/crawfordxx/xiaoma-studio) (MIT, © xiaoma-studio contributors), commit `6f7b492`: `SKILL.md`, `references/aesthetics.md`, `references/storyboard.md`, `references/motion.md`, `templates/STORYBOARD.md`.

Changes by Mosio: translated from Chinese, condensed and adapted to Mosio's composition rules (paused GSAP timeline, no audio, no CDN) for Indonesian small businesses on Reels, TikTok, Shorts and WhatsApp Status. The render engine, CLIs, audio, music, voice, bilingual subtitles, image and video models, web and slide parts were dropped, and the 10 original archetypes were reduced to five looks mapped to Mosio styles; added the ending rule, WhatsApp Status, preview scoring and the social formats (tips, behind the scenes, before/after, series, trends). Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
