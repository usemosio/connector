# Footage edit: the user's own video, cut into a post

Use when the user has filmed something and wants it edited: a talking head (tips, a review, a testimonial, an announcement), a product or process montage (making the coffee, packing orders, a shop tour), an event or behind-the-scenes clip, or a short cut from a long recording. The craft is in `mosio/video-composition/craft/`: `editing.md` (cuts, rhythm, sound), `reframe.md` (landscape to vertical), `zooms.md`, `subtitles.md`, `text-effects.md`.

## 1. The footage

- `list_media` lists what is ready (path, length, size, sound). Not there yet: `upload_media` shows the upload card in the chat, where the user picks the clips from their phone or computer; the card posts their upload ids once they are in. Attachments ChatGPT passes as files go through `import_media`; agents with a shell send local files with `create_media_upload` and `complete_media_upload`. Many files at once: the dashboard's Media page (`upload_url`). Each clip takes a minute or two to prepare; wait for it in `list_media`. Never pretend to have footage.
- Look at every clip you might use with `inspect_media` (numbered frames with times, scene cuts, silences, and what was said). Before asking anything, tell the user in one line per clip what you see and hear, so they can correct you: "Klip 1 (0:42): kakak bicara ke kamera di depan etalase, bahas kopi susu gula aren; klip 2 (0:15): close-up kopi dituang, ada potongan di 0:06."
- You cannot hear the clip, but `inspect_media` gives an automatic transcript (phrases with times; `words` for each word's start and end). It can mishear names, prices and brand words: check those with the user before they go on screen. The user's own script or SRT wins over it. A file still being transcribed says so: inspect it again in a few minutes.
- A clip the user only wants the look of ("bikin kayak video ini") is a reference, not footage: it is studied, never placed (`../brief.md`, "A reference video"). Unsure which: ask once.
- Footage of other people (customers, staff) or other creators' content: ask whether the user has permission to post it. Never use clips of TV, other creators' videos or music the user does not own.

## 2. Brief: the questions this type adds

In the one batch of questions from `brief.md`, ask only what the clips and the brand do not already answer:

- the goal and the one action after watching (and where it is posted);
- the length and size (default 9:16, 15-30 s for a montage or promo, 20-45 s for talking content);
- moments that must stay and moments to cut ("yang bagian ketawa jangan dibuang");
- subtitles yes or no, and the spoken words if yes;
- music: their own uploaded track, or the clip's own sound only (Mosio adds no music).

## 3. The edit plan, then wait

Write the plan in plain words, 4-8 sentences, and wait for the user's OK (after "langsung aja", go straight to the preview but still write the plan): the shape (hook, body, ending), which shots in which order (by clip, frame number and time), what gets cut, where zooms and text land, the subtitle style, the music level, and the estimated length. `preview_storyboard` is optional here: the plan is the storyboard, with beats as roles (hook, proof, step, offer, cta). A recipe from `list_recipes` only styles the graphics layer (titles, lower thirds, end card); the footage stays the picture.

## 4. Structures

| Kind                            | Length                       | Structure                                                                                                                                                                  |
| ------------------------------- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Talking head (tips, opinion)    | 20-45 s                      | hook line in the first 2 s (no greeting, no logo intro), points as short sections each opening with a mini-hook, CTA; the speaker on screen at least about 65% of the time |
| Testimonial                     | 15-30 s                      | the strongest sentence first, who they are (name pill), the problem and the result in their words, the product, CTA; captions on                                           |
| Product or process montage      | 10-20 s                      | the finished product or the best action frame first, 4-8 process shots of 0.4-2 s on a rhythm, the hero shot held 2-3 s, offer and CTA on the end card                     |
| Shop tour or event              | 15-30 s                      | arrival or the exterior, 3-5 highlights, one quiet moment, the address and opening hours on the end card                                                                   |
| Short cut from a long recording | 15-55 s (21-34 s for TikTok) | one complete idea that stands alone: opens with its hook, ends on its payoff, no "seperti yang tadi saya bilang"                                                           |

- The hook: the strongest moving frame plus a short text hook at the top. Never open on a still talking head or a slow pan; start mid-sentence with energy, or with a 0.5-1 s flash of the best b-roll.
- Talking content, per line: the speaker alone (the opening, opinions, stories, the CTA), the speaker with an overlay toward the edges (a keyword, a price counting up, an arrow), or a full cover (b-roll, a chart or steps) while the voice carries on. No stretch of speaker-only longer than about 8 s; covers run 3-8 s and return to the speaker between them; full-screen b-roll at most about a quarter of the runtime.
- Overlay timing: structure (cards, axes) enters about 6 frames early; a key fact (a price, a percentage) lands on the word that says it (its start from the transcript's `words`; otherwise on the phrase start); a punchline's reveal waits for the word.
- The ending holds the CTA, name and contact at least 2 s (the guide's rule), over a held frame of the footage or the brand background.
- Text behind the speaker (a big word their head and shoulders overlap, the hook or the price): `make_cutout` on that stretch (at most 6 s), then base clip, text and cutout clip as in `text-effects.md` 6a. Use it once per video, where it lands the point.

## 5. Picking a short cut from a long recording

From the transcript (or the user's own text), score each candidate passage of 15-55 s: hook strength 30% (a bold claim, a curiosity gap, a specific number in the first 3 s), stands alone 25% (no references to earlier parts, ends on a full sentence), emotion 20% (conviction, surprise, humour), value 15% (steps, a specific tip, real numbers), payoff 10% (a punchline or a clear takeaway). Propose the top 2-3 with times and the reason; the user picks. Without speech (montages, music only), pick from the frames, and say so.

## 6. Credits and truth

- A render costs its length in credits: an edit of a 3-minute recording into a 30 s post costs 30 s. Offer the shorter version before a long one.
- Never claim the footage shows something it does not, and never put words in a speaker's mouth: on-screen quotes and subtitles are their words. Prices and claims on screen only from the user or the brand.
- Label any illustration added over real footage ("Ilustrasi") when it could be taken as real.

## Adapted from

- [browser-use/video-use](https://github.com/browser-use/video-use) (MIT, © Browser Use), commit `43cfc56`: `SKILL.md` (inventory, describe what you see, questions shaped by the material, a 4-8 sentence strategy confirmed before editing, structural archetypes).
- [AgriciDaniel/claude-shorts](https://github.com/AgriciDaniel/claude-shorts) (MIT, © Daniel Agrici), commit `a369fad`: `SKILL.md`, `references/scoring-rubric.md` (the five weighted dimensions, 15-55 s segments, shorter for TikTok, standalone red flags).
- [iart-ai/b-roll-skills](https://github.com/iart-ai/b-roll-skills) (MIT, © iart.ai), commit `a51e34b`: `skills/talking-head-b-roll/SKILL.md` (speaker, overlay or cover per line; the rhythm limits; lead structure by about six frames, sync facts to the word, wait for the reveal).
- [kurbaitaev/ghost-editor](https://github.com/kurbaitaev/ghost-editor) (MIT, © kurbaitaev), commit `8b9a3c5`: `references/broll.md` (budgets: full-screen b-roll at most 25% of the runtime, the speaker on screen at least 65%).
- [kmilo93sd/agent-video-editor](https://github.com/kmilo93sd/agent-video-editor) (MIT, © kmilo93sd), commit `8e32c0a`: `skills/editing-videos/recipes/talking-head.md` (hook without a greeting or logo intro, sections that open with a mini-hook).

Changes by Mosio: rewritten as a Mosio video type for small businesses and creators: footage uploaded from the chat or the workspace's Media page, eyes through `inspect_media`, spoken words from Mosio's automatic transcript (checked with the user) or the user's own script, the edit plan as the storyboard, recipes only for the graphics layer, Mosio's credits, consent and truth rules. Dropped the sources' own transcription steps, face tracking, generated b-roll and narration, scripts and render commands. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
