# Brief and storyboard

Turn "buatkan video" into a short brief and a content storyboard in chat, before any HTML or recipe call. The guide's rules still apply and win on conflict. Start straight with the questions or the brief: no preamble, no explaining this skill.

## Order

1. Read the brand first: `list_brands`; with several, ask which one unless the request already names it (a prompt from the dashboard carries `brand_id`); then `get_brand` with that `brand_id`. What it sets is already answered (business, voice, colours, fonts, logo, contact, products and prices): show it in the brief and never ask it again. When the user's message gives a different value (a promo price, a new number), the user's value wins: use it, note the difference in one line, and do not ask about it. What it lists under "Not set" (or marks "not set; ask the user") goes on the question list. Pass the same `brand_id` to every tool that makes the video.
2. Take everything the user already said (product, price, topic, platform, duration, liked examples), then ask what is missing in **one batch of at most 5 questions** and wait.
3. Write the brief and the content storyboard, show it with `preview_storyboard`, and wait for approval and a recipe pick. Then write the recipe plan and take the path chosen in `SKILL.md`.

## Needed before the storyboard

| Field    | Accepted answer                                                                                              | Examples (promo / tips / social post)                                                                  |
| -------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| Core     | one thing sold, taught or shown, with real numbers (the brand's products and prices when it is one of them)  | "Es kopi susu gula aren, Rp18.000" / "3 cara kemeja putih tetap putih" / "Roti dipanggang jam 4 subuh" |
| For whom | one specific person, and where they watch                                                                    | office workers on Reels at lunch / moms washing at home, TikTok / the bakery's followers, IG Stories   |
| Action   | one action                                                                                                   | "Pesan lewat WhatsApp" / "Simpan video ini" / "Pesan untuk besok pagi"                                 |
| Contact  | exactly as customers use it (from the brand when set)                                                        | WA number, @account, Shopee store name, short address                                                  |
| Size     | from where it is posted (the guide's sizes)                                                                  | Reels and TikTok: 9:16                                                                                 |
| Duration | seconds                                                                                                      | 15 s                                                                                                   |
| Feel     | at most 3: warm, premium, cheerful, urgent, calm, bold, modern (the brand's voice and motion style when set) | "hangat, ceria"                                                                                        |

You may suggest size and duration (default 9:16, 10-20 s) if marked as a suggestion. Prices, numbers, contacts, addresses, hours, testimonials, ratings and claims in a lesson (doses, results, times) come only from the user or the brand.

## Questions: one short batch

Owners are busy and answer on a phone. One message, numbered, at most 5, each answerable in one line. Factual questions (size, duration) get options with your suggestion first. Creative questions (feel, core message) stay open with one example, so you do not steer the answer.

```
Biar videonya pas, 4 hal dulu:
1. Menu mana yang mau ditonjolkan? Di brand ada Es Kopi Susu Gula Aren (Rp18.000) dan Es Kopi Pandan (Rp20.000).
2. Siapa yang paling sering beli? (misal: mahasiswa sekitar kampus)
3. Setelah nonton, orang harus ngapain: chat WA, datang, atau pesan di GoFood?
4. Diunggah di mana? Saran saya Reels/TikTok, vertikal 15 detik.
Nama usaha, warna, kontak, dan gaya bahasa saya ambil dari brand Kopi Senja.
```

For a tips video, ask instead for the points in the owner's own words and whether each is true for their business. Empty answer: ask once more. Still empty: **never guess**. Write `[belum ada]` in the brief and leave that part out of the video (an ending without a number beats an invented one).

## Push to shot level

Adjectives and estimates are not answers. Offer two picturable options and their effect. One extra round only; do not interrogate.

| User says                         | Reply with                                                                                                                                    |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| "Yang kekinian aja"               | "Kekinian yang mana: kata besar bergantian cepat seperti promo sneakers, atau putih bersih dengan satu produk besar dan banyak ruang kosong?" |
| "Sekitar 15 detikan"              | "Saya kunci 15 detik: 5 bagian, satu ide per bagian. Oke?"                                                                                    |
| "Buat semua orang"                | "Siapa yang paling sering beli? Satu saja, nanti bahasanya saya sesuaikan."                                                                   |
| "Biar viral"                      | "Kalau berhasil, penonton melakukan apa: chat WA, datang, simpan, atau follow?"                                                               |
| "Masukin semua menu / semua tips" | "15 detik muat 3 yang terbaca. Mana 3 andalan? Sisanya bisa jadi video kedua."                                                                |
| "Pokoknya ada diskon"             | "Diskonnya berapa, dari harga berapa, dan sampai kapan?"                                                                                      |
| "Terserah kamu"                   | Decide, list it under "Saran saya" with a one-line reason. Facts are still never guessed.                                                     |

**Core message**: one sentence, at most 7 words, that works as a poster headline ("Kopi gula aren asli, Rp18.000", "Kemeja putih tanpa pemutih"). Three messages? Pick one with the user; the others are other videos.
**Examples from other brands**: take the motion only, never their logo, copy or colors. Colors come only from typed hex values or the brand; from a blurry screenshot, ask.

## Combined check

Before the storyboard, read all answers together and flag problems that only appear combined, in one sentence with a fix.

- 6 s but 4 products or 5 tips: one item, or 15 s. "Calm" for a flash sale: "bold", or two videos.
- 1:1 ending with address, number and hours: WA number only. 16:9 for WhatsApp status: 9:16.
- Price and discount disagree (Rp100.000 to Rp70.000 called "diskon 50%"): ask which is right.

## Brief format

Short. Separate what came from the user or the brand from your suggestions.

```
Brief
- Inti: Es kopi susu gula aren, Rp18.000
- Untuk: mahasiswa sekitar kampus, Reels sore hari
- Pesan inti: Kopi gula aren asli, Rp18.000
- Ajakan: Pesan lewat WhatsApp · Kontak: [dari brand]
- Ukuran: 9:16, 1080×1920 · Durasi: 15 detik · Rasa: hangat, santai
Dari kamu dan brand: inti, untuk, ajakan, kontak, warna.
Saran saya (perlu konfirmasi): 9:16 15 detik untuk Reels; sketsa recipe Produk utama (product-hero) dan Kinetic type karena satu produk dengan harga.
```

## Storyboard in two layers

**Layer 1, the content storyboard**, is what the user approves. It is recipe-neutral: it says what each beat says and shows, never how it moves.

- 3-6 beats (6-8 s: 3, 10-15 s: 4-5, 20-30 s: 5-6; the tool takes at most 8). One idea per beat.
- **Role**, one of: `hook` (stop the scroll), `problem` (the pain), `fact` (one statement), `step` (one step of a how-to), `compare` (two things: before and after, myth and fact, old and new price), `number` (one real figure), `proof` (a review, a detail, a result), `offer` (what is sold and the deal), `cta` (the one action and contact), `payoff` (the closing answer or lesson). The type file lists the usual sequence. Start with `hook`; end with `cta` when selling, `payoff` when teaching or telling.
- **On-screen text**: exactly as it will show, at most 7 words, in quotes.
- **Key word**: the word in that text where the beat lands.
- **Data**: the facts or numbers from the user or the brand that the beat uses. Nothing invented; missing means `[belum ada]` and the beat goes without it.
- **Subject**: what is shown, an object or idea ("the iced coffee glass"), not how it moves.
- **Weight**: `short`, `medium` or `long`. Hook usually short, ending long. The chosen recipe's tempo turns weights into seconds later.
- No motion or transition words ("slams in", "wipe", "zoom", "counts up") and no exact seconds: those belong to layer 2.

Promo example (illustrative; prices and contacts come from the user):

| #   | Role   | On-screen text                | Key word  | Data                     | Subject                         | Weight |
| --- | ------ | ----------------------------- | --------- | ------------------------ | ------------------------------- | ------ |
| 1   | hook   | "Ngantuk jam 2 siang?"        | Ngantuk   |                          | a sleepy afternoon at the desk  | short  |
| 2   | offer  | "Es Kopi Susu Gula Aren"      | Gula Aren |                          | the iced coffee glass           | medium |
| 3   | proof  | "Gula aren asli, bukan sirup" | asli      | real palm sugar (user)   | a palm sugar block by the glass | medium |
| 4   | number | "Cuma Rp18.000"               | Rp18.000  | Rp18.000 (user)          | the price                       | medium |
| 5   | cta    | "Pesan via WA · Kopi Senja"   | WA        | WA number from the brand | the order button and contact    | long   |

Education example ("3 cara kemeja putih tetap putih"): `hook` "Kemeja putihmu mulai kusam?", three `step` beats, each with the user's own instruction in Data (e.g. "rendam air dingin 30 menit") and the shirt as the subject, then a `payoff` "Simpan, coba minggu ini".

## Show it and pick a recipe

1. Call `preview_storyboard` with a short title, the beats, and in `recipes` 1-3 recipes that fit the content (how to pick them below). When the user already chose a recipe (a prompt from the dashboard's Recipes names it, e.g. "dengan recipe Papan menu (menu-board)"), send only that one and offer another only if asked. It draws each beat as a hand-drawn sketch panel per recipe, with that recipe's timing. It is free and cheap: you send data, not HTML. Without a card (terminal, scripts), the same beats as a table in chat are enough.
2. Ask in one line: "Isinya sudah pas? Mau recipe yang mana?" (one recipe: "Isinya sudah pas?"). The user approves the content and picks a recipe, in the card or in chat.
3. The card shows each recipe's total length. Far from the agreed duration: change weights or the number of beats, show again.

Nearest recipe: `list_recipes` says what each recipe is for (its description, tags and size); pick by the content, the size and the feel, and `types/<type>.md` names the usual starting point for its type. Fill-in path: the chosen fill-in recipe plays the role of the recipe.

## Layer 2, the recipe plan

After approval and the recipe pick, call `get_composition_guide` with that `recipe`. Its "Recipe plan" section maps each role to a technique and gives the tempo: seconds per weight, endings at least 2 s. The plan sets the structure and the timing; the brand still wins on colours, fonts, logo, words and motion feel (the guide's "Recipe and brand").

- Each beat gets its role's technique, and its weight becomes seconds. Seconds are contiguous: each beat starts where the last ended, and the total becomes `data-duration`.
- Add the motion and transitions here, never in layer 1: the event lands on the key word, and each beat moves into the next (wipe, mask, zoom, slide, words out before new words in), never a plain appear and disappear. Craft: `mosio/video-composition/craft/`.
- Frame 0 is already composed or moving. Text to read holds at least 1.2 s. The last beat holds at least 2 s and exits in the final 0.5 s.
- Do not rewrite the content: text, key words, data and order stay as approved. If the plan seems to need a content change, ask first.
- Each beat start becomes a timeline label, e.g. `tl.addLabel("b3", 4.6)`, with beat 3 tweens at `"b3"` or `"b3+=0.2"`.

## Revisions

- A revision is not approval: show the new version and ask again.
- **Content edit** (text, data, order, a beat added or removed, a weight): change only those beats, call `preview_storyboard` again, ask again.
- **Recipe change**, now or after the preview: keep the beats as they are and redo only the plan, with `get_composition_guide` for the new recipe. No new content approval.
- **A second recipe**: offer it, never build it unasked. Read only the chosen recipe's guide.
- **Several sizes**: same beats and plan, layout rebuilt per size. Offer first: each size costs its own credits.
- Big-word rhythm: `skill_library` path `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md`.

## Adapted from

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills), commit `229022e`: `skills/brand-intake/SKILL.md`, `skills/motion-brief-writer/SKILL.md`.
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub), commit `9e73275`: `SKILL.md`, `references/workflow-0-1.md`, `references/question-bank.md`, `references/scene-breakdown.md`, `references/pacing-rules.md`, `templates/video-spec-template.md`.
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors), commit `220fa2a`: `hyperdirector/prompts/02-intake-brief.md`, `hyperdirector/prompts/03-storyboard-generator.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes/references/brief-contract.md`, `skills/hyperframes/references/intent-interview.md`, `skills/hyperframes/references/storyboard-format.md`.

Changes by Mosio: translated and condensed into one chat flow (no brand.md, MOTION.md, video-spec.md, BRIEF.md or JSON files), questions cut to one batch of at most 5 for small businesses, and the workspace brand from `get_brand` used first (its products, prices and contact are never asked again). Dropped voice, TTS, 3D, shaders, asset search and the 4-phase score; durations and text rules follow Mosio's guide (no audio, paused GSAP timeline, 2 s held ending), and examples cover Indonesian promos, tips and social posts. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
