# Brief and storyboard

Turn "buatkan video" into a short brief and a second-by-second storyboard in chat, before any HTML or template call. The guide's rules still apply and win on conflict. Start straight with the questions or the brief: no preamble, no explaining this skill.

## Order

1. Call `get_composition_guide` first. Its brand kit (business name, main color, background, contact) is already answered: show it in the brief, never ask it again. Fields marked "(belum diisi)" go on the question list.
2. Take everything the user already said (product, price, topic, platform, duration, liked examples), then ask what is missing in **one batch of at most 5 questions** and wait.
3. Write the brief and storyboard, ask "Sudah pas, atau ada yang diubah?", and after approval take the path chosen in `SKILL.md`.

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
| "Sekitar 15 detikan"              | "Saya kunci 15 detik: 4 adegan, masing-masing 3 sampai 4 detik. Oke?"                                                                         |
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
Saran saya (perlu konfirmasi): 9:16 15 detik untuk Reels; gaya product-hero karena satu produk dengan harga.
```

## Storyboard format

- 3-6 scenes (6-8 s: 3, 10-15 s: 4-5, 20-30 s: 5-6). Seconds with one decimal, contiguous: each scene starts where the last ended; the total equals the duration and later `data-duration`.
- Scene 1 is the hook, 0-1.5 s: big motion, short text; not a lone logo, not a title fading in from an empty screen. Body scenes 2-4 s, one idea each, and each has one event (something changes from start to end). Text to read holds at least 1.2 s.
- The last scene has the action, name and contact, holds at least 2 s, and exits in the final 0.5 s.
- On-screen text exactly as it will appear, at most 7 words at once.
- Motion as verb plus easing feel ("slams in", "counts up then bounces", "revealed from behind a line"). Transition: how it moves to the next scene (wipe, mask, zoom, slide, words out before new words in), never a plain appear and disappear.

| #   | Seconds   | On-screen text                         | Motion                                                 | Transition                  |
| --- | --------- | -------------------------------------- | ------------------------------------------------------ | --------------------------- |
| 1   | 0.0-1.5   | "Ngantuk jam 2 siang?"                 | words slam in one by one, big to normal                | words exit upward           |
| 2   | 1.5-4.5   | "Es Kopi Susu Gula Aren"               | SVG glass rises, background gradient drifts            | diagonal wipe in main color |
| 3   | 4.5-8.0   | "Cuma Rp18.000"                        | price counts up, lands with a small bounce             | zoom out of the number      |
| 4   | 8.0-11.5  | "Gula aren asli, bukan sirup"          | two lines revealed from behind a line, "asli" accented | slide left                  |
| 5   | 11.5-15.0 | "Pesan via WA · Kopi Senja · [kontak]" | button enters, then holds; fade out 14.5-15.0          | end                         |

Illustrative only; prices and contacts come from the user. Below the table: "Total 15,0 detik. Sudah pas, atau ada yang diubah?"

## Approval, revisions, hand-off

- A revision is not approval: show the new version and ask again. Changing one scene: change only it and shift later seconds so the total holds.
- Several sizes: same storyboard, layout rebuilt per size. Offer first: each size costs its own credits.
- Each scene start becomes a timeline label, e.g. `tl.addLabel("s3", 4.5)`, with scene 3 tweens at `"s3"` or `"s3+=0.2"`. Timeline duration equals the storyboard total.
- Nearest style: one product and price `product-hero`, discount `flash-sale`, slogan `kinetic-type`, menu `menu-board`, testimonial `chat-testimoni`, numbers `angka-capaian`, app `peluncuran-aplikasi`, ad services `dasbor-iklan`, wide explainer `explainer-isometrik`. Big-word rhythm: `skill_library` path `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md`.

## Adapted from

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills), commit `229022e`: `skills/brand-intake/SKILL.md`, `skills/motion-brief-writer/SKILL.md`.
- [feicaiclub/video-spec-builder](https://github.com/feicaiclub/video-spec-builder) (MIT, © feicaiclub), commit `9e73275`: `SKILL.md`, `references/workflow-0-1.md`, `references/question-bank.md`, `references/scene-breakdown.md`, `references/pacing-rules.md`, `templates/video-spec-template.md`.
- [gloweaseco-leo/hyperdirector](https://github.com/gloweaseco-leo/hyperdirector) (Apache-2.0, © HyperDirector Contributors), commit `220fa2a`: `hyperdirector/prompts/02-intake-brief.md`, `hyperdirector/prompts/03-storyboard-generator.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes/references/brief-contract.md`, `skills/hyperframes/references/intent-interview.md`, `skills/hyperframes/references/storyboard-format.md`.

Changes by Mosio: translated and condensed into one chat flow (no brand.md, MOTION.md, video-spec.md, BRIEF.md or JSON files), questions cut to one batch of at most 5 for small businesses, and the workspace brand kit from `get_composition_guide` used first. Dropped voice, TTS, 3D, shaders, asset search and the 4-phase score; durations and text rules follow Mosio's guide (no audio, paused GSAP timeline, 2 s held ending), and examples cover Indonesian promos, tips and social posts. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
