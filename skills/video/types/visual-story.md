# Visual story

A short vertical explainer that feels like a film, not slides: one illustrated world, a camera that moves through it, and the story told word by word in captions. It answers one "why" or "how come" question with a true number and a payoff. Run `../brief.md` first; technical rules come from `get_composition_guide`. Starting code: the `visual-story` style (`get_composition_guide` with `style: "visual-story"`), a silent 2D film about why bees build hexagons. Craft: `../../video-composition/craft/captions.md` (the caption engine) and `../../video-composition/craft/camera.md` (the 2D camera, flights, breakdowns).

## When to use

- One question answered like a short film: a science fact, why a process works, what happens inside a product ("Kenapa sarang lebah bentuknya segi enam?").
- It needs a subject you can draw and a number that is true: computed from a formula, counted in the picture, or given by the user.
- Steps, how-to, tips and myth vs fact are an Explainer (`explainer.md`). A visual story is one question, one answer, one world.

## Silent by design

- Mosio renders silent videos. **The captions are the voice**: every word of the script appears on screen, word by word, at reading pace.
- No voice-over, TTS or generated audio, and do not offer one. If the user wants narration, they can record it and add it in the app they post to, or later when Mosio supports user-supplied audio. The approved script then doubles as their VO script.
- True 3D (three.js, WebGL) is not available in Mosio today. Depth comes from 2D pseudo-depth: SVG isometric shapes, a parallax camera and shading (`camera.md`).

## Script first (gate)

| #   | Part                           | Bees example                                                           |
| --- | ------------------------------ | ---------------------------------------------------------------------- |
| 1   | Question hook                  | "Kenapa sarang lebah bentuknya segi enam?"                             |
| 2   | Rule or fact                   | "Cuma ada tiga bentuk yang bisa nutup rapat tanpa celah."              |
| 3   | A computable number            | "Buat nampung madu yang sama, segi enam butuh dinding paling sedikit." |
| 4   | Consequence                    | "Lilinnya lebih hemat, madunya lebih banyak."                          |
| 5   | Payoff that flips the question | "Lebah nggak perlu rumus. Segi enam udah paling efisien."              |
| 6   | CTA (optional)                 | name and contact, the guide's ending held 2 s or more                  |

- **One sentence = one beat = one visual metaphor.** 5-8 beats, 18-30 s in total, inside the plan's limit.
- **Write natively per language.** A casual register reads well ("nggak", "udah", "Cuma ada tiga bentuk yang bisa nutup rapat"). Never translate a pun literally: "The shape does" becomes "Segi enam udah paling efisien", not "Bentuknya yang berhitung".
- Short sentences: about 10 words at most, so each splits into phrases of 1-5 tokens (`captions.md`).
- **Facts are true, numbers are computed.** Show the working in the brief: equal-area cells (area 1) have perimeters 4,56 (triangle), 4,00 (square), 3,72 (hexagon), so the hexagon needs about 7% less wall than the square. No source and not common knowledge: ask, or keep the claim qualitative. Never invent.
- **Gate: script approval before building.** Show the script phrase by phrase with the boxed tokens marked, plus the beat plan below. This is the storyboard of `../brief.md`; wait for a yes before any HTML.

## Beats

For each sentence, plan one row:

| Sentence (anchor token)    | Technique                                      | Tag or counter                |
| -------------------------- | ---------------------------------------------- | ----------------------------- |
| hook ("segi enam?")        | push-in on the bee closing the last wall       | none                          |
| rule ("celah:")            | hero cell flies into three tilings, seams glow | "UJI" tag                     |
| number ("paling sedikit.") | cells explode into wall bars, bars tint        | 4,56 / 4,00 / 3,72, "dinding" |
| payoff ("Segi enam")       | pull back to the finished comb (bookend)       | none                          |

- Usual roles: hook → fact → compare → number → payoff.
- **Transitions, in order of preference**: a camera move inside the same world; a shared-element flight into the next picture; an in-place state change (a wall closes, a glow, a strike-through); a flat-to-isometric tilt, when depth is earned; a zoom fly-through with a colour flash; a dissolve with a small blur, only between unrelated worlds.
- **Every motion lands on its word**: an event's time is its token's time from the caption plan (`at("token")`), within 2 frames. Nothing important moves during a phrase gap; idle motion (wing flaps, drift) always runs.
- Answer "what's inside" or "why" by opening the object already on screen (a breakdown), never by cutting to a new diagram.

## 2D first

- The hook is always 2D and reads in one glance: rich illustration (gradients, texture, parallax, particles, soft shadows), not flat icons. Lesson learned: a 3D-looking box seen from the side in second one read as "a yellow box"; a comb on a branch with a bee building a cell read instantly.
- **One world, a moving camera.** Never chain flat pages with dissolves. Elements change roles: the thing that matters flies into its next job.
- Pseudo-depth (isometric tilt, extrusion) only when the idea needs it: a viewpoint change, going inside, a volume. Usually 1-3 middle beats. When you go into depth, the usual way in is to make the drawing already on screen stand up and lift the key element out of it, rather than cutting to new objects (see `../../video-composition/craft/camera.md` § Depth in 2D); use judgement, it is not required every time. End on the hook's own illustration, completed: the bookend closes the film.

## Layout zones (9:16, 1080 × 1920)

| Zone     | y (px)    | Content                                                                                                                                                |
| -------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Top safe | 0-250     | art and scrim only                                                                                                                                     |
| Caption  | 280-500   | word-by-word caption, 2 lines max, centred, 920 px wide                                                                                                |
| Tag      | 520-590   | small pill tag with 1-3 words ("UJI · madu sama")                                                                                                      |
| Hero     | 620-1300  | the one visual metaphor of the beat                                                                                                                    |
| Counter  | 1300-1420 | big number, small lowercase label                                                                                                                      |
| Footnote | 1760-1830 | "Ilustrasi · tidak sesuai skala", 40 px, left or centred: the one text allowed in the bottom zone, since nothing breaks if a platform button covers it |
| Bottom   | 1500-1920 | the guide's bottom safe zone: world art may continue; no text except the footnote                                                                      |

Other sizes: rebuild the zones, never scale (`../../video-composition/craft/sizes.md`).

## Silent pacing

| Item             | Value                                                                 |
| ---------------- | --------------------------------------------------------------------- |
| Reading pace     | 2.3-2.8 words per second (default 2.5)                                |
| Phrase           | 1-5 tokens, broken at commas and sentence ends                        |
| Phrase gap       | 0.25-0.4 s (0.4 s after a sentence end)                               |
| Emphasised token | about one per 1-2 sentences, held 0.2-0.3 s longer                    |
| Phrase on screen | at least 1.2 s from its first token                                   |
| Beat             | one sentence, 2.5-4 s                                                 |
| Hook             | the world is on screen at frame 0, the first token by 0.3 s           |
| Ending           | the payoff phrase holds, then the ending (or last frame) 2 s or more  |
| Words on screen  | the guide's 7-word cap covers caption, tag and counter label together |

## Colour and type

- **Warm dark world**: `radial-gradient` #2a1d0e to #0d0a07 with a faint 40 px grid (about 2% white). A light world (#fafafa, grid about 3.5% black) is allowed between worlds.
- **Semantic colour**, the same meaning for the whole film. The colour change tells the story: three wall bars tint red, amber, green on "paling sedikit.".

| Colour                  | Means                                  |
| ----------------------- | -------------------------------------- |
| gold #ffc53d            | the hero quantity, the subject (honey) |
| red #e5484d / #ff6369   | worst, danger                          |
| amber #f5a524           | middle                                 |
| green #30a46c / #3dd68c | best, correct                          |
| slate #a7b0c4           | "the others"                           |

- Type: Inter (Google Fonts, 400/700/900) for captions, tags and counters; concept words living in the scene in Playfair Display italic (`vendor/`). Two families at most, so formulas use Inter with `tabular-nums`.
- Counters start on the number's token, count 0.5-0.8 s with `power2.out`, weight 900, semantic colour, id-ID format ("4,56"; `../../video-composition/craft/numbers.md`).
- **Footnote** "Ilustrasi · tidak sesuai skala" ("Illustration · not to scale" in English) on every illustrative scene: 40 px, about 60% opacity, centred in its zone.

## Do not

- Put a background box behind the whole caption, sweep karaoke colour, bounce words, or put emoji in captions.
- Use three.js, WebGL or a canvas over text; force depth, or open on an object from an unusual angle.
- Cut to a new diagram to show "what's inside"; open the object instead.
- Invent numbers or facts, or use stock photos of real people as heroes.
- Write a voice-over, call TTS, or suggest generating audio.

## Before you send

- `preview_composition` until it passes; fix from `details`.
- **Keyword check**: list each anchor token with its planned time and the event placed there. Every event time comes from `at()`, never a typed number. Watch those exact moments in the preview card: the box, glow, count or strike lands on its word.
- Framing per beat: the subject in the hero zone, nothing behind the caption, counters clear of the hero, nothing cut at the edges, safe zones clear.
- Transitions: no pops from nothing, no labels before their objects, no jump at a hand-off.
- Numbers match the computation; the footnote is on illustrative scenes; the ending holds 2 s or more. Then say honestly what you checked.

## Adapted from

Original to Mosio: adapted from the team's own visual-story skill (the bees and hexagon explainer). Changes by Mosio: made silent (captions carry the story, timed at reading pace, no voice-over or TTS), 2D only (no three.js or WebGL), 30 fps, and fitted to Mosio's composition rules and tools; the Python, CLI and voice pipeline was dropped.
