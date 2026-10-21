# Explainer: education without a voice

Education videos: how it works, 3 steps, myth vs fact, a glossary term, "did you know". Mosio renders are **silent**, so the explainer is carried by text and visuals alone: no narration, no voice, no audio. Run `../brief.md` first; technical rules come from `get_composition_guide`.

## Core rule

A source text is an information dump; a video is a guided act of understanding. Never paraphrase the user's notes in paragraph order. Reorder, merge, cut. Before the storyboard, write down:

- **Audience**: who watches and what they already know.
- **Gap**: the confusion or "why care" the video resolves.
- **Thesis**: the one line the viewer leaves with. It lands by the second screen.
- **Spine**: 3-5 steps, mechanisms or items that build to the thesis.
- **Landing**: the recap, then what to try or do.

Facts, numbers and examples come from the user. No source? Keep the claim qualitative and label worked examples "Contoh" on screen. Health, finance and legal topics: stay general and suggest the user checks the facts.

## Pick one structure

| Type              | Shape                                                                                            | Visual stage                   |
| ----------------- | ------------------------------------------------------------------------------------------------ | ------------------------------ |
| How it works      | name the idea, reveal the mechanism layer by layer, land the "so what"                           | one SVG diagram that grows     |
| Steps (3 langkah) | one step per screen, in order, same object carried across steps                                  | one stage, a numbered stepper  |
| Myth vs fact      | myth in the opponent colour, labelled "MITOS", struck; fact replaces it bigger, labelled "FAKTA" | split, or same spot swapped    |
| Glossary          | term big, plain definition in 7 words or fewer, one example                                      | type as the hero               |
| Did you know      | surprising claim or number, why it happens, what it means for the viewer                         | number or metaphor as the hero |

Usual roles: hook → step → step → step → payoff. Myth vs fact: hook → compare → fact → payoff.

One structure per video. Two topics means two videos. For myth vs fact, the fact gets the larger type and the last word on screen.

## Hook (first 2 s, text and motion)

Open a gap, never a definition. Shapes that work silent, 7 words max: a question ("Kenapa roti cepat berjamur?"), a counter-intuitive claim ("Cuci piring air panas belum tentu bersih"), pain validation ("Bingung bedanya PPN dan PPh?"), a concept announcement ("Namanya: harga pokok penjualan"), a scenario ("Bayangkan stok habis pas ramai"). A number only if the user gave it.

## Silent pacing rules

- **One concept or step per screen.** The next appears only after the previous one has been read.
- **Reading time**: text holds still at least **1.2 s + 0.25 s per word**. "Rendam 10 menit" (3 words) holds about 2 s; with a 0.5 s entrance and 0.8 s before the transition, that step is about 3.3 s.
- **Numbered progress cue** in a fixed spot for the whole body: a "1/3" chip, dots or a stepper bar that fills one segment per step (tween `scaleX` on the fill, `transform-origin: left center` in CSS).
- **Recap screen at the end**: all steps or the thesis in one view, 7 words max (icons plus one-word labels: "Rendam · Kucek · Bilas"), held at least the reading time. Then the guide's ending (name, CTA, contact) held 2 s or more.
- Typical length: hook 2 s, 3 steps of about 3.5 s, recap 3 s, ending 2.5 s, so 18-20 s. Stay inside the plan's limit; cut items before squeezing reading time.

## Visuals

- **Diagrams in inline SVG**: nodes, arrows, a number line, a simple chart. Draw lines with `DrawSVGPlugin` (from `vendor/`), information lines 2 px or more.
- **Build complexity step by step.** Screen 1 shows one node; each later screen adds one part. Never show the finished diagram first.
- **Labels sit next to what they name**, not in a legend. A short leader line at most.
- **One consistent stage**: consecutive steps keep the same diagram, position and colours, and use the same seam each time. If one element turns into another (a node becomes a bar), do it inside one scene, not across a cut.
- The hero visual fills 40-60% of the frame. For glossary and "did you know", type is the hero: near full-bleed, one emphasised word.
- Invent a metaphor from the content (a funnel, a staircase, a scale), not generic decoration. No purple-blue "tech" gradients, no fake UI unless the topic is that UI.
- Emphasis: a marker sweep, a hand-drawn circle or the accent colour on the one word that matters.

## Motion

- Smooth long-tail settles (`power3.out`, `expo.out`) by default. Bounce and overshoot only for a deliberately playful moment.
- Don't front-load: at t=0 only the first piece enters; the rest wait for their turn. A held still screen is fine; a screen dumped then frozen is not.
- No looping "breathing" on cards or text and no slow pan or push during reading. Stillness beats bad motion. Background aliveness stays faint (guide).
- Seams between steps: a velocity-matched cut. Outgoing hero `x: 0 to -230` over 0.3 s `power4.in`, fading out early; incoming `fromTo` from `x: 230` to 0, same duration, `power4.out`. For a chapter change, a zoom-through on the heading wrapper with blur 10 px or less on that small element only (CPU render, never full-screen). More: `../../video-composition/craft/transitions.md`, `../../video-composition/craft/motion.md`.

## Sizes: the `explainer-isometrik` style

`explainer-isometrik` is 16:9 (1920x1080): text in the left half, an isometric SVG stage on the right, problem then workload then solution and a chart. It uses `bounce.out` and `back.out` on entrances; switch them to `power3.out` for a calm explainer.

For **9:16**, rebuild the layout, don't scale it: set `data-width="1080" data-height="1920"`, the viewport meta and `html`/`body` to 1080x1920. Stack vertically: heading on top, below the guide's top safe zone; the SVG stage under it, about 900 px wide (roughly y 550 to 1350); the progress cue just above the bottom safe zone. Shorter heading lines, bigger type. Side-by-side splits (myth vs fact) become top/bottom. Keep the same scene order and timings. See `../../video-composition/craft/sizes.md`.

## Before you send

- One structure, one thesis; order follows understanding, not the source text.
- Every screen holds one concept, still for 1.2 s + 0.25 s per word; the progress cue advances once per step.
- The diagram grows one part per screen, labels beside their parts; the hero is 40-60% of the frame.
- A recap screen, then the ending held 2 s or more. Every fact and number comes from the user; examples labelled "Contoh".

More in `skill_library`: steps on one canvas `hyperframes/hyperframes-animation/blueprints/spatial-pan-stations.md`, myth vs fact `hyperframes/hyperframes-animation/blueprints/comparison-split.md`, parts around one idea `hyperframes/hyperframes-animation/blueprints/constellation-hub.md`, a number reveal `hyperframes/hyperframes-animation/blueprints/dataviz-countup.md`, line drawing `hyperframes/hyperframes-animation/rules/svg-path-draw.md`, markers `hyperframes/hyperframes-animation/rules/css-marker-patterns.md`. Translate any audio or voice cues there into reading-time beats.

## Adapted from

- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/faceless-explainer/SKILL.md`, `skills/faceless-explainer/references/story-design.md`, `skills/faceless-explainer/references/visual-design.md`, `skills/faceless-explainer/references/motion-language.md`, `skills/faceless-explainer/references/cut-catalog.md`.

Changes by Mosio: condensed and adapted to Mosio's composition rules (one HTML file, paused GSAP timeline, no audio, no CDN). Narration, TTS, music, sound effects, captions, CLIs, scripts, frame presets and sub-agent workflow were dropped; voiceover-paced reveals became reading-time pacing (1.2 s plus 0.25 s per word), and Mosio added the education formats, the numbered progress cue, the recap screen and the 9:16 rebuild of the `explainer-isometrik` style. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
