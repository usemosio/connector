# Ad: paid Meta and TikTok video

For videos that will run with budget on Meta Ads (Facebook, Instagram) or TikTok Ads, where every second and every click is paid. Run `../brief.md` first, then fill the scenes with the structure here. Technical rules come from `get_composition_guide`.

## Two rules that make an ad testable

1. **Hook and CTA are a pair.** The promise in the first 3 s is paid off by the CTA at the end, and by the place people land after the click. Hook "Kemeja kusut tiap Senin?" closes with "Chat WA, kemeja rapi besok pagi", not "Belanja sekarang".
2. **One variant, one change.** Testing the hook? Only the hook differs. Layout, colour, motion, length and CTA stay the same, or the winner cannot be read.

## Anatomy

| Beat    | Job                                            | 15 s  | 30 s  |
| ------- | ---------------------------------------------- | ----- | ----- |
| Hook    | stop the thumb: problem, surprise or promise   | 0-2.5 | 0-3   |
| Context | make the viewer feel the problem               | 2.5-5 | 3-8   |
| Payoff  | product as the answer, one clear benefit       | 5-9   | 8-18  |
| Proof   | one real number, demo or result                | 9-12  | 18-25 |
| CTA     | one action, matches the hook, held 2 s or more | 12-15 | 25-30 |

- The hook trigger (key word or main image) must read before 2 s; viewers decide in one or two seconds.
- 6 s bumper: hook 0-2, payoff 2-4, CTA 4-6. Drop context and proof.
- Length guide: Meta Feed 6-15 s, Reels and Stories 8-15 s, TikTok In-Feed 15-30 s. Mosio default 10-20 s.
- The hook moves cost per result most. Test hooks first, then offer, then CTA.

## Hook: one line, one image

Mosio renders are silent and most people watch ads muted. The hook has two parts that complement, not repeat:

- **Visual motion** at 0-2 s: product drops into frame, price gets struck, a chat arrives, a creased shirt turns crisp.
- **On-screen text**: the claim, 7 words max, big, inside the safe zone.

Never let the text describe the picture ("Ini kemeja" over a shirt). The image shows the state; the text names the consequence.

| Type           | Shape                   | Example (illustrative, fit to the user's facts) | Use when                     |
| -------------- | ----------------------- | ----------------------------------------------- | ---------------------------- |
| Problem first  | name a felt problem     | "Kemeja kusut tiap Senin pagi?" (laundry)       | felt but rarely said         |
| Pattern break  | unexpected line         | "Jangan pesan katering ini dulu"                | crowded, look-alike category |
| Curiosity gap  | hold the answer         | "Bahan rahasia sambal kami: bukan cabai"        | sceptical, curious viewers   |
| Direct benefit | measurable result       | "Laporan stok beres dalam 5 menit" (POS app)    | result is clear and true     |
| Call-out       | name the segment        | "Yang jualan di Shopee, berhenti rekap manual"  | sharp segment                |
| Question       | what buyers type        | "Cari kue ulang tahun di Depok?"                | local search                 |
| POV            | very specific situation | "POV: tanggal tua, lapar, dompet tipis"         | young viewers, cheap product |

Numbers in a hook (5 menit, 6 jam) only if the user gave them and they are true. More patterns and how to diagnose a failing hook: `skill_library` `copywriting/ad-creative/references/hook-system.md`.

## Message match: hook, CTA, landing

| Hook promises                  | CTA in video                   | Must exist after the click                       |
| ------------------------------ | ------------------------------ | ------------------------------------------------ |
| "Kemeja rapi besok pagi"       | "Chat WA, jemput hari ini"     | WA admin ready to answer pickup and price per kg |
| "Kue ulang tahun di Depok"     | "Pesan di Shopee: [nama toko]" | the birthday cake listing, not the shop home     |
| "Gratis ongkir se-Jabodetabek" | "Klik, ongkir kami tanggung"   | free-shipping terms visible on the listing       |

- Product name, price and promo must match the WhatsApp chat or listing exactly; a small mismatch loses the sale.
- For click-to-WhatsApp ads, suggest a greeting message that continues the hook's promise.
- Contact and shop name come from the brand kit or the user. No URL needed on screen when the ad button already links there.

## Honest proof

The strongest proof can be checked: units sold, ratings, real testimonials, the user's own before/after photos, a demo of the product working. All of it **only from the user**. No data? Show the process (dough, washing machine, app screen) or skip the proof beat. Styles: `chat-testimoni` for reviews, `angka-capaian` for numbers, `peluncuran-aplikasi` for app demos, `dasbor-iklan` for ad or agency results. Health, beauty and finance claims are restricted by Meta and TikTok policy: avoid outcome promises ("kulit putih 7 hari") and suggest the user checks the policy.

## Readable with sound off

- The whole story works muted: hook, benefit and CTA all exist as on-screen text, with sizes and contrast from the guide (`Mosio.readableOn`).
- 9:16 uses the guide's safe zones. On TikTok the right-side icon column covers roughly 140 px (device-dependent): keep key text centred, not hugging the right.
- Keep hook, product and CTA inside the central 1080x1080 square so 4:5 and 1:1 versions only need re-layout, not cropping (see `../../video-composition/craft/sizes.md`).
- No music or voice-over in the plan. If the user wants music, add it in Ads Manager or the TikTok editor after render.

## Variants for A/B tests

One composition, one swapped part. Mark the tested element so the next variant only changes that text:

```html
<!-- VARIANT: hook. Everything else is identical across variants. -->
<h1 id="hook" class="hook">Kemeja kusut tiap Senin pagi?</h1>
<!-- VARIANT: CTA (tested in round two) -->
<div id="ajak" class="ajak">Chat WA, kemeja rapi besok</div>
```

Test order: (1) **hook**, 3-5 hooks of different types with the same body and CTA; (2) **offer or benefit** with the winning hook; (3) **CTA** with the winning hook and offer, still matching the hook; (4) **size**, the winner in 9:16, 4:5 and 1:1.

- Same length and timeline; only text changes. Check the longest hook still fits (7 words, inside the safe zone).
- Preview each variant with `preview_composition`. Each render costs credits equal to its length: offer the number of variants and sizes before making them all.
- Title by variable, e.g. "Laundry Kilat, hook 2: pattern break", so Ads Manager results are easy to match.
- If the CTA changes along with the hook, keep the pair matched and note the test is of the pair, not the hook alone.

## Before you send

- Hook trigger reads before 2 s; the first scene moves from frame one.
- CTA pays off the hook and matches the WhatsApp chat or listing.
- Each variant differs in one part only. Every number, price, testimonial and contact comes from the user or brand kit.
- Fully understood muted; text in the safe zone and the central square. Ending held 2 s or more; timeline length equals `data-duration`.

## Adapted from

- [iart-ai/ad-video-skills](https://github.com/iart-ai/ad-video-skills) (MIT, © iart.ai), commit `0de0f2a`: `skills/ad-creative-video/SKILL.md`, `skills/ad-creative-video/references/platform-specs.md`, `skills/ad-creative-video/references/batch-ad-pipeline.md`.

Changes by Mosio: translated, condensed and adapted to Mosio's composition rules (one HTML file, paused GSAP timeline, no audio, no CDN). Remotion code, the CSV parser and batch render scripts were dropped and replaced by variant markers in the HTML and per-variant previews; hook examples were rewritten for Indonesian small businesses, and message match to WhatsApp and marketplaces plus honest-proof rules were added. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
