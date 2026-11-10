# Brief and storyboard

Turn "buatkan video" into a short brief and a content storyboard in chat, before any HTML or recipe call. The guide's rules still apply and win on conflict. Start straight with the questions or the brief: no preamble, no explaining this skill.

## Order

1. Know the goal first (`SKILL.md`, Flow), then read the brand: `list_brands`; with several, ask which one unless the request already names it (a prompt from the dashboard carries `brand_id`); then `get_brand` with that `brand_id`. What it sets is already answered (business, voice, colours, fonts, logo, contact, products and prices): show it in the brief and never ask it again. When the user's message gives a different value (a promo price, a new number), the user's value wins: use it, note the difference in one line, and do not ask about it. What it lists under "Not set" (or marks "not set; ask the user") goes on the question list. Pass the same `brand_id` to every tool that makes the video. No brand and the video is a story, a lesson or a creator's post: go on without one and never ask for a business name.
2. Take everything the user already said (product, price, topic, platform, duration, liked examples) and what the workspace already holds (`get_workspace_context`, `list_notes`: research from another app, saved Discover posts, figures), then ask only what is still missing in **one batch of at most 5 questions** and wait. Asking for what a note already says makes the user repeat work they did elsewhere.
3. Write the brief and save it with `save_production_plan`; link the notes it came from with `promote_notes`. Then the look (`recommend_recipes`), the content storyboard for it, `preview_storyboard`, and wait for approval. Then write the recipe plan and build as `SKILL.md` says.

## Needed before the storyboard

| Field    | Accepted answer                                                                                              | Examples (promo / tips / social post)                                                                  |
| -------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| Core     | one thing sold, taught or shown, with real numbers (the brand's products and prices when it is one of them)  | "Es kopi susu gula aren, Rp18.000" / "3 cara kemeja putih tetap putih" / "Roti dipanggang jam 4 subuh" |
| For whom | one specific person, and where they watch                                                                    | office workers on Reels at lunch / moms washing at home, TikTok / the bakery's followers, IG Stories   |
| Ending   | what the last beat leaves, by goal (table below)                                                             | "Pesan lewat WhatsApp" / "Simpan video ini" / "Deg-deg itu pintu yang ditutup darah"                   |
| Contact  | only when the video sells or launches: exactly as customers use it (from the brand when set)                 | WA number, @account, Shopee store name, short address                                                  |
| Size     | from where it is posted (the guide's sizes)                                                                  | Reels and TikTok: 9:16                                                                                 |
| Duration | only when the user or the platform fixes one (an ad slot, "pas 15 detik"); otherwise the recipe sets it      | "pas 15 detik" / (not asked: the recipe's tempo)                                                       |
| Feel     | at most 3: warm, premium, cheerful, urgent, calm, bold, modern (the brand's voice and motion style when set) | "hangat, ceria"                                                                                        |

You may suggest the size if marked as a suggestion (9:16 by default). Do not propose a length: the chosen recipe's tempo turns the beats' weights into seconds and the storyboard card shows the total, so a number picked before the recipe would fight it. When the user or the platform fixes a length, keep it and fit the beats and weights to it; with a narrated video, the recording sets it. Every length stays inside the plan's limit. Prices, numbers, contacts, addresses, hours, testimonials, ratings and claims in a lesson (doses, results, times) come only from the user or the brand.

### By goal

| Goal                               | Needed before the storyboard                                                                                                       | Ending (the last beat, held 2 s or more)                           |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Sell (promo, ad, menu, service)    | the product, its real price, the one action, the contact                                                                           | `cta`: the action and the contact                                  |
| Launch something new               | what is new, the one reason to care, proof the user gave (a spec, a demo, a detail), when and where it is available                | `cta` or `payoff`: the reveal with the date and the place          |
| Story or narrated video            | the question or conflict, who or what it is about, the payoff (the answer that flips what the viewer expected), each fact's source | `payoff`, then at most one small action (save it, try it yourself) |
| Lesson, or one episode of a series | one question per episode, the points in the user's words, a source for every number                                                | `payoff`, then the series name or the next episode's teaser        |
| Announcement                       | what changes, from when                                                                                                            | the change and the date; a contact only if people must act         |

A story, a lesson or a series has no price and needs no contact: never ask for them, and end on the payoff, not a sales line. Label every number in the brief by where it comes from: the user, official (a standard, a datasheet, a manual, a textbook), media, or modelled (computed for the picture: shown as an illustration, never as a fact).

## Questions: one short batch

Owners are busy and answer on a phone. One message, numbered, at most 5, each answerable in one line. Factual questions (size, duration) get options with your suggestion first. Creative questions (feel, core message) stay open with one example, so you do not steer the answer.

```
Biar videonya pas, 4 hal dulu:
1. Menu mana yang mau ditonjolkan? Di brand ada Es Kopi Susu Gula Aren (Rp18.000) dan Es Kopi Pandan (Rp20.000).
2. Siapa yang paling sering beli? (misal: mahasiswa sekitar kampus)
3. Setelah nonton, orang harus ngapain: chat WA, datang, atau pesan di GoFood?
4. Diunggah di mana? Saran saya Reels/TikTok, vertikal.
Nama usaha, warna, kontak, dan gaya bahasa saya ambil dari brand Kopi Senja.
```

A story or a lesson asks about the question and the facts instead of the product and the contact:

```
Biar ceritanya kena, 5 hal dulu:
1. Satu pertanyaan apa yang dijawab videonya? (misal: kenapa jantung bunyi deg-deg)
2. Di akhir penonton harus merasa apa: kaget, paham, atau penasaran episode berikutnya?
3. Fakta dan angkanya dari mana? Yang belum ada sumbernya saya tandai sebagai ilustrasi.
4. Mau pakai suaramu sendiri (rekam naskahnya di HP) atau teks saja?
5. Diunggah di mana? Saran saya TikTok/Reels, vertikal.
```

For a tips video, ask instead for the points in the owner's own words and whether each is true for their business. Empty answer: ask once more. Still empty: **never guess**. Write `[belum ada]` in the brief and leave that part out of the video (an ending without a number beats an invented one).

## Push to shot level

Adjectives and estimates are not answers. Offer two picturable options and their effect. One extra round only; do not interrogate.

| User says                         | Reply with                                                                                                                                                     |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "Yang kekinian aja"               | "Kekinian yang mana: kata besar bergantian cepat seperti promo sneakers, atau putih bersih dengan satu produk besar dan banyak ruang kosong?"                  |
| "Sekitar 15 detikan"              | "Saya kunci 15 detik, dan isinya saya sesuaikan. Oke?"                                                                                                         |
| "Buat semua orang"                | "Siapa yang paling sering beli? Satu saja, nanti bahasanya saya sesuaikan."                                                                                    |
| "Biar viral"                      | "Kalau berhasil, penonton melakukan apa: chat WA, datang, simpan, atau follow?" For a story: "Di akhir mereka harus merasa apa: kaget, paham, atau penasaran?" |
| "Pokoknya edukatif"               | "Satu pertanyaan apa yang dijawab? Satu kalimat."                                                                                                              |
| "Bikin yang bercerita"            | "Ceritanya tentang siapa atau apa, dan di akhir apa yang berubah?"                                                                                             |
| "Masukin semua menu / semua tips" | "Satu video muat 3 yang terbaca. Mana 3 andalan? Sisanya bisa jadi video kedua."                                                                               |
| "Pokoknya ada diskon"             | "Diskonnya berapa, dari harga berapa, dan sampai kapan?"                                                                                                       |
| "Terserah kamu"                   | Decide, list it under "Saran saya" with a one-line reason. Facts are still never guessed.                                                                      |

**Core message**: one sentence, at most 7 words, that works as a poster headline ("Kopi gula aren asli, Rp18.000", "Kemeja putih tanpa pemutih"). Three messages? Pick one with the user; the others are other videos.
**Examples from other brands** (a video or screenshots the user wants it to look like): follow "A reference video" below. Take the structure and motion only, never their logo, copy or colors. Colors come only from typed hex values or the brand; from a blurry screenshot, ask.

## Combined check

Before the storyboard, read all answers together and flag problems that only appear combined, in one sentence with a fix.

- A short length the user fixed (6 s) but 4 products or 5 tips: one item, or a longer video. "Calm" for a flash sale: "bold", or two videos.
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
- Ukuran: 9:16, 1080×1920 · Durasi: ikut tempo recipe · Rasa: hangat, santai
Dari kamu dan brand: inti, untuk, ajakan, kontak, warna.
Saran saya (perlu konfirmasi): 9:16 untuk Reels; sketsa recipe Produk utama (product-hero) dan Kinetic type karena satu produk dengan harga.
```

A story brief has no offer or contact (illustrative):

```
Brief (cerita)
- Pertanyaan: Kenapa jantung bunyi deg-deg?
- Untuk: pelajar dan orang awam, TikTok
- Payoff: deg-deg itu bunyi katup yang ditutup darah, bukan bunyi otot
- Fakta: ±70 ml darah per detak (resmi: buku anatomi) · "sekitar ⅓ gelas" (dimodelkan, ditandai ilustrasi)
- Penutup: payoff, lalu "coba dengar di dada kiri temanmu"
- Suara: suaramu sendiri (rekam naskah) · Ukuran: 9:16 · Durasi: panjang rekamannya
Dari kamu: pertanyaan, untuk, suara. Saran saya (perlu konfirmasi): payoff, penutup.
```

## Concept: the look before the beats

A recipe is not a skin. Each brings a world and a kind of subject (one object, an illustrated world, characters, a conversation, an object's layers, type only, data, the user's footage), how much text a beat holds, and a tempo. Its plan in `get_composition_guide` and its description in `list_recipes` show which: "the subject stays, a numbered caption swaps below it" wants one object; "one short bubble per step" a conversation; "one part of the drawing per step, the camera following" an illustrated world. Beats written before the look is known fit some recipes and break in others, so the concept comes first.

- **A quick sale whose facts decide it** (one product, its price, a contact): the concept is the 1-3 recipes with the same kind of subject that you send with the storyboard. No separate step.
- **A story, a lesson, a launch, or a user who is unsure**: offer 2-3 concepts in one message. Each is a recipe plus the world or metaphor, the main subject, the 3-second hook, and captions only or the user's voice. They differ in idea, not only in colour (for the heart: a house of four rooms whose doors slam / a chalkboard diagram of the flow / one big counter of millilitres per beat). Recommend one in a line; `show_recipes` can show the recipes moving.
- The pick is **locked**: the storyboard is written for that recipe (its kind of subject, text per beat and tempo; narrated, the recording's length), and `preview_storyboard` sketches it in that recipe.

## A reference video

A video the user wants theirs to look like ("kayak gini", "referensinya ini", "contohnya") is a reference: you study it, and it never goes into the video.

- **Getting it in.** A public social video link goes through `import_reference`; wait for preparation, then `inspect_media` gives stills, cuts, timing and transcript. If the platform refuses it, ask for the file or 3-6 screenshots of key moments. A video file comes in through the upload card (`upload_media`; `import_media` where the chat passes attachments as files), then `inspect_media` gives its numbered stills, hard cuts, silences and words. Screenshots attached in the chat you can see directly; they do not need to be in the media library. A file attached in the chat is not in the media library until it comes in one of those ways.
- **Reference or footage?** When it is not clear whether the user wants the file on screen (their own footage: `types/footage-edit.md`) or only its look, ask once. A reference never appears in the composition.
- **Break it down** in 4-6 lines before any concept: its length and number of scenes (from the hard cuts), the seconds per scene, where the hook lands, how text appears (size, words at once, how it enters), camera and transitions, colour logic, how it ends, and whether it has a voice or music. Say what stills cannot show (the motion between them, the exact easing), and do not guess it.
- **The concepts start from it** ("Concept"): one as close as possible (the nearest recipe by kind of subject and motion, given the reference's number of scenes and rhythm), one with the reference's structure in another look, and one different idea. Name the recipe each builds on. When no recipe is close, the closest concept is composed from the breakdown alone: say that its look is new, and sketch the storyboard in the nearest recipe (the card needs one).
- **The storyboard inherits its rhythm**: the same number of beats where the content allows, weights matched to the reference's scene lengths, the hook at the same moment.
- **What may be taken**: the structure, the rhythm, kinds of motion, the camera grammar, layout ideas. **Never**: its logo, text or slogans, characters or mascots, footage, music or voice, a distinctive illustration, or one of its frames as is. Colours come from the brand or typed hex values, never sampled from the reference. The reference file is never placed in the composition (no `assets/media/` path of it).

## Storyboard in two layers

**Layer 1, the content storyboard**, is what the user approves. It says what each beat says and shows, never how it moves, and its subjects live in the chosen concept's world.

- 3-6 beats (the tool takes at most 8). One idea per beat. Fewer, longer beats beat many rushed ones: each beat must leave time to read its line. With a length the user fixed, fit the count to it.
- **Role**, one of: `hook` (stop the scroll), `problem` (the pain), `fact` (one statement), `step` (one step of a how-to), `compare` (two things: before and after, myth and fact, old and new price), `number` (one real figure), `proof` (a review, a detail, a result), `offer` (what is sold and the deal), `cta` (the one action and contact), `payoff` (the closing answer or lesson). The type file lists the usual sequence. Start with `hook`; end with `cta` when selling, `cta` with the date or a `payoff` when launching, `payoff` when teaching or telling.
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

Story example ("Kenapa jantung bunyi deg-deg?"): `hook` "Deg-deg itu bukan suara otot?", `fact` "4 ruang, 4 pintu", two `step` beats (the first and the second valve closing), `number` "Sekali deg: ±70 ml" with its source in Data, then a `payoff` "Deg-deg = pintu ditutup darah". No `offer`, no `cta`, no contact.

## Show it and pick a recipe

1. Call `preview_storyboard` with a short title, the beats, and in `recipes` the chosen concept's recipe, or for a quick sale the 1-3 recipes with the same kind of subject (how to pick them below; required). When the user already chose a recipe (a prompt from the dashboard's Recipes names it, e.g. "dengan recipe Papan menu (menu-board)"), send only that one. The card draws each beat as a hand-drawn sketch panel of its content, then offers your recipes as looks, each playing in the brand's colours, with every other recipe behind "Lihat recipe lain"; under each panel it shows how the picked look moves that beat and when. It is free and cheap: you send data, not HTML. Without a card (terminal, scripts), the same beats as a table in chat are enough.
2. Ask in one line: "Isinya sudah pas? Mau tampilan yang mana?" (one recipe: "Isinya sudah pas?"). The user approves the content and picks a recipe, in the card or in chat; the pick may be one you did not name. A pick with another kind of subject is a recipe change ("Revisions").
3. The card shows each recipe's total length. Far from the agreed duration: change weights or the number of beats, show again.

Nearest recipe: `list_recipes` says what each recipe is for (its description, tags and size); pick by the content, the size and the feel, and `types/<type>.md` names the usual starting point for its type.

## Layer 2, the recipe plan

After approval and the recipe pick, call `get_composition_guide` with that `recipe`. Its "Recipe plan" section maps each role to a technique and gives the tempo: seconds per weight, endings at least 2 s. The plan sets the structure and the timing; the brand still wins on colours, fonts, logo, words and motion feel (the guide's "Recipe and brand").

- Each beat gets its role's technique, and its weight becomes seconds. Seconds are contiguous: each beat starts where the last ended, and the total becomes `data-duration`.
- Add the motion and transitions here, never in layer 1: the event lands on the key word, and each beat moves into the next (wipe, mask, zoom, slide, words out before new words in), never a plain appear and disappear. Craft: `mosio/video-composition/craft/`.
- Frame 0 is already composed or moving: feeds autoplay from frame 0 and often take it as the default cover, so an empty first frame looks like a broken video and gets scrolled past. Text to read holds at least 1.2 s, the time a short line needs to be read once on a phone. The last beat holds at least 2 s, so the contact can be read or screenshotted, and exits in the final 0.5 s.
- Do not rewrite the content: text, key words, data and order stay as approved. If the plan seems to need a content change, ask first.
- Each beat start becomes a timeline label, e.g. `tl.addLabel("b3", 4.6)`, with beat 3 tweens at `"b3"` or `"b3+=0.2"`.

## Revisions

- A revision is not approval: show the new version and ask again.
- **Content edit** (text, data, order, a beat added or removed, a weight): change only those beats, call `preview_storyboard` again, ask again.
- **Recipe change**, now or after the preview. Read the new recipe's plan with `get_composition_guide` first, then:
  - **Same kind of subject** (one big-type look for another, one product look for another): keep the beats as they are and redo only the plan. No new content approval.
  - **Another kind of subject** (one product to an illustrated world, a conversation or characters): keep every fact and approved line exactly (text, prices, contact, numbers, order); rewrite only each beat's subject for the new world and its weight for the new tempo; show which beats changed in a short table; sketch again with `preview_storyboard` in the new recipe only; and ask once: "Isinya tetap; visualnya berubah di beat 1, 2 dan 4. Oke?" Never restart the brief.
  - **A video already edited in Studio**: say first that those edits stay with the old version and do not carry over to the new recipe.
- **A second recipe**: offer it, never build it unasked. Read only the chosen recipe's guide.
- **Several sizes**: same beats and plan, layout rebuilt per size. Offer first: each size costs its own credits.
- Big-word rhythm: `skill_library` path `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md`.

## Adapted from

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills), commit `229022e`: `skills/brand-intake/SKILL.md`, `skills/motion-brief-writer/SKILL.md`.
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub), commit `9e73275`: `SKILL.md`, `references/workflow-0-1.md`, `references/question-bank.md`, `references/scene-breakdown.md`, `references/pacing-rules.md`, `templates/video-spec-template.md`.
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors), commit `220fa2a`: `hyperdirector/prompts/02-intake-brief.md`, `hyperdirector/prompts/03-storyboard-generator.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes/references/brief-contract.md`, `skills/hyperframes/references/intent-interview.md`, `skills/hyperframes/references/storyboard-format.md`.

Changes by Mosio: translated and condensed into one chat flow (no brand.md, MOTION.md, video-spec.md, BRIEF.md or JSON files), questions cut to one batch of at most 5 for small businesses, and the workspace brand from `get_brand` used first (its products, prices and contact are never asked again). Dropped voice, TTS, 3D, shaders, asset search and the 4-phase score; durations and text rules follow Mosio's guide (no audio, paused GSAP timeline, 2 s held ending), and examples cover Indonesian promos, tips and social posts. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
