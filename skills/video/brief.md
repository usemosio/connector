# Brief and storyboard

Turn "buatkan video" into a short brief and a content storyboard in chat, before any HTML or template call. The guide's rules still apply and win on conflict. Start straight with the questions or the brief: no preamble, no explaining this skill.

## Order

1. Call `get_composition_guide` first. Its brand kit (business name, main color, background, contact) is already answered: show it in the brief, never ask it again. Fields marked "(belum diisi)" go on the question list.
2. Take everything the user already said (product, price, topic, platform, duration, liked examples), then ask what is missing in **one batch of at most 5 questions** and wait.
3. Write the brief and the content storyboard, show it with `preview_storyboard`, and wait for approval and a style pick. Then write the style plan and take the path chosen in `SKILL.md`.

## Needed before the storyboard

| Field    | Accepted answer                                                | Examples (promo / tips / social post)                                                                  |
| -------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Core     | one thing sold, taught or shown, with real numbers             | "Es kopi susu gula aren, Rp18.000" / "3 cara kemeja putih tetap putih" / "Roti dipanggang jam 4 subuh" |
| For whom | one specific person, and where they watch                      | office workers on Reels at lunch / moms washing at home, TikTok / the bakery's followers, IG Stories   |
| Action   | one action                                                     | "Pesan lewat WhatsApp" / "Simpan video ini" / "Pesan untuk besok pagi"                                 |
| Contact  | exactly as customers use it                                    | WA number, @account, Shopee store name, short address                                                  |
| Size     | from where it is posted (the guide's sizes)                    | Reels and TikTok: 9:16                                                                                 |
| Duration | seconds                                                        | 15 s                                                                                                   |
| Feel     | at most 3: warm, premium, cheerful, urgent, calm, bold, modern | "hangat, ceria"                                                                                        |

You may suggest size and duration (default 9:16, 10-20 s) if marked as a suggestion. Prices, numbers, contacts, addresses, hours, testimonials, ratings and claims in a lesson (doses, results, times) come only from the user or the brand kit.

## Questions: one short batch

Owners are busy and answer on a phone. One message, numbered, at most 5, each answerable in one line. Factual questions (size, duration) get options with your suggestion first. Creative questions (feel, core message) stay open with one example, so you do not steer the answer.

```
Biar videonya pas, 5 hal dulu:
1. Menu apa yang mau ditonjolkan, dan harganya berapa?
2. Siapa yang paling sering beli? (misal: mahasiswa sekitar kampus)
3. Setelah nonton, orang harus ngapain: chat WA, datang, atau pesan di GoFood?
4. Diunggah di mana? Saran saya Reels/TikTok, vertikal 15 detik.
5. Kesan yang dicari, maks 3 kata (misal: hangat, santai)?
Nama usaha dan warna saya ambil dari brand kit: Kopi Senja, kuning dan biru gelap.
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
**Examples from other brands**: take the motion only, never their logo, copy or colors. Colors come only from typed hex values or the brand kit; from a blurry screenshot, ask.

## Combined check

Before the storyboard, read all answers together and flag problems that only appear combined, in one sentence with a fix.

- 6 s but 4 products or 5 tips: one item, or 15 s. "Calm" for a flash sale: "bold", or two videos.
- 1:1 ending with address, number and hours: WA number only. 16:9 for WhatsApp status: 9:16.
- Price and discount disagree (Rp100.000 to Rp70.000 called "diskon 50%"): ask which is right.

## Brief format

Short. Separate what came from the user or brand kit from your suggestions.

```
Brief
- Inti: Es kopi susu gula aren, Rp18.000
- Untuk: mahasiswa sekitar kampus, Reels sore hari
- Pesan inti: Kopi gula aren asli, Rp18.000
- Ajakan: Pesan lewat WhatsApp · Kontak: [dari brand kit]
- Ukuran: 9:16, 1080×1920 · Durasi: 15 detik · Rasa: hangat, santai
Dari kamu dan brand kit: inti, untuk, ajakan, kontak, warna.
Saran saya (perlu konfirmasi): 9:16 15 detik untuk Reels; sketsa gaya product-hero dan kinetic-type karena satu produk dengan harga.
```

## Storyboard in two layers

**Layer 1, the content storyboard**, is what the user approves. It is style-neutral: it says what each beat says and shows, never how it moves.

- 3-6 beats (6-8 s: 3, 10-15 s: 4-5, 20-30 s: 5-6; the tool takes at most 8). One idea per beat.
- **Role**, one of: `hook` (stop the scroll), `problem` (the pain), `fact` (one statement), `step` (one step of a how-to), `compare` (two things: before and after, myth and fact, old and new price), `number` (one real figure), `proof` (a review, a detail, a result), `offer` (what is sold and the deal), `cta` (the one action and contact), `payoff` (the closing answer or lesson). The type file lists the usual sequence. Start with `hook`; end with `cta` when selling, `payoff` when teaching or telling.
- **On-screen text**: exactly as it will show, at most 7 words, in quotes.
- **Key word**: the word in that text where the beat lands.
- **Data**: the facts or numbers from the user or brand kit that the beat uses. Nothing invented; missing means `[belum ada]` and the beat goes without it.
- **Subject**: what is shown, an object or idea ("the iced coffee glass"), not how it moves.
- **Weight**: `short`, `medium` or `long`. Hook usually short, ending long. The chosen style's tempo turns weights into seconds later.
- No motion or transition words ("slams in", "wipe", "zoom", "counts up") and no exact seconds: those belong to layer 2.

Promo example (illustrative; prices and contacts come from the user):

| #   | Role   | On-screen text                | Key word  | Data                     | Subject                         | Weight |
| --- | ------ | ----------------------------- | --------- | ------------------------ | ------------------------------- | ------ |
| 1   | hook   | "Ngantuk jam 2 siang?"        | Ngantuk   |                          | a sleepy afternoon at the desk  | short  |
| 2   | offer  | "Es Kopi Susu Gula Aren"      | Gula Aren |                          | the iced coffee glass           | medium |
| 3   | proof  | "Gula aren asli, bukan sirup" | asli      | real palm sugar (user)   | a palm sugar block by the glass | medium |
| 4   | number | "Cuma Rp18.000"               | Rp18.000  | Rp18.000 (user)          | the price                       | medium |
| 5   | cta    | "Pesan via WA · Kopi Senja"   | WA        | WA number from brand kit | the order button and contact    | long   |

Education example ("3 cara kemeja putih tetap putih"): `hook` "Kemeja putihmu mulai kusam?", three `step` beats, each with the user's own instruction in Data (e.g. "rendam air dingin 30 menit") and the shirt as the subject, then a `payoff` "Simpan, coba minggu ini".

## Show it and pick a style

1. Call `preview_storyboard` with a short title, the beats, and 1-3 styles that fit the content (nearest style list below). It draws each beat as a hand-drawn sketch panel per style, with that style's timing. It is free and cheap: you send data, not HTML. Without a card (terminal, scripts), the same beats as a table in chat are enough.
2. Ask in one line: "Isinya sudah pas? Mau gaya yang mana?" The user approves the content and picks a style, in the card or in chat.
3. The card shows each style's total length. Far from the agreed duration: change weights or the number of beats, show again.

Nearest style: one product and price `product-hero`, discount `flash-sale`, slogan `kinetic-type`, menu `menu-board`, testimonial `chat-testimoni`, numbers `angka-capaian`, app `peluncuran-aplikasi`, ad services `dasbor-iklan`, wide explainer `explainer-isometrik`, "why" story `visual-story`. Template path: the chosen template plays the role of the style.

## Layer 2, the style plan

After approval and the style pick, call `get_composition_guide` with that `style`. Its "Style plan" section maps each role to a technique and gives the tempo: seconds per weight, endings at least 2 s.

- Each beat gets its role's technique, and its weight becomes seconds. Seconds are contiguous: each beat starts where the last ended, and the total becomes `data-duration`.
- Add the motion and transitions here, never in layer 1: the event lands on the key word, and each beat moves into the next (wipe, mask, zoom, slide, words out before new words in), never a plain appear and disappear. Craft: `../video-composition/craft/`.
- Frame 0 is already composed or moving. Text to read holds at least 1.2 s. The last beat holds at least 2 s and exits in the final 0.5 s.
- Do not rewrite the content: text, key words, data and order stay as approved. If the plan seems to need a content change, ask first.
- Each beat start becomes a timeline label, e.g. `tl.addLabel("b3", 4.6)`, with beat 3 tweens at `"b3"` or `"b3+=0.2"`.

## Revisions

- A revision is not approval: show the new version and ask again.
- **Content edit** (text, data, order, a beat added or removed, a weight): change only those beats, call `preview_storyboard` again, ask again.
- **Style change**, now or after the preview: keep the beats as they are and redo only the plan, with `get_composition_guide` for the new style. No new content approval.
- **A second style**: offer it, never build it unasked. Read only the chosen style's guide.
- **Several sizes**: same beats and plan, layout rebuilt per size. Offer first: each size costs its own credits.
- Big-word rhythm: `skill_library` path `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md`.

## Adapted from

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills), commit `229022e`: `skills/brand-intake/SKILL.md`, `skills/motion-brief-writer/SKILL.md`.
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub), commit `9e73275`: `SKILL.md`, `references/workflow-0-1.md`, `references/question-bank.md`, `references/scene-breakdown.md`, `references/pacing-rules.md`, `templates/video-spec-template.md`.
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors), commit `220fa2a`: `hyperdirector/prompts/02-intake-brief.md`, `hyperdirector/prompts/03-storyboard-generator.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes/references/brief-contract.md`, `skills/hyperframes/references/intent-interview.md`, `skills/hyperframes/references/storyboard-format.md`.

Changes by Mosio: translated and condensed into one chat flow (no brand.md, MOTION.md, video-spec.md, BRIEF.md or JSON files), questions cut to one batch of at most 5 for small businesses, and the workspace brand kit from `get_composition_guide` used first. Dropped voice, TTS, 3D, shaders, asset search and the 4-phase score; durations and text rules follow Mosio's guide (no audio, paused GSAP timeline, 2 s held ending), and examples cover Indonesian promos, tips and social posts. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
