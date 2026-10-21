# One brief, several sizes

Use when the same video is needed in several places: Reels and the Instagram feed, TikTok and YouTube, WhatsApp status and an in-store screen. The sizes, their safe zones and the one-composition-per-size rule are in the guide (Sizes; Layout and safe zones); this file is how to do it cleanly.

## 1. Offer first, never make them all at once

- Each size renders separately and **each is charged its own duration**: three sizes of a 15 s video use 45 s of credit.
- Ask where the video will be posted and suggest only the sizes that place needs (the guide's Sizes table maps places to sizes).
- Cheapest flow: design and approve one main size first (usually 9:16) until the user is happy with its preview. Then derive the others one at a time, each through `preview_composition`.

## 2. Principle: frame = f(time, seed, size)

Every frame must be computable from three things only: timeline time, the PRNG seed, and the canvas size.

- **Same timing in every size.** Scene order, time ranges, `data-duration` and every tween position are identical. Only the layout changes.
- **Same seed in every size.** Random decoration (dots, particles, confetti) uses the same seeded mulberry32, so the pattern matches across versions; positions are fractions of width and height, not fixed pixels.
- **Size enters only through layout constants.** Scenes never write their own pixel numbers; they read one `LAYOUT` object chosen from `data-width` × `data-height`.
- All four Mosio sizes have a 1080 px short side, so font sizes in px can stay the same (the guide's type sizes); what changes is the long side and the arrangement.

## 3. Re-lay out, never scale or crop

Scaling the 9:16 version to 16:9 leaves small text with empty sides; cropping throws text away. Arrange per size:

| Size | Arrangement                                                                                                     |
| ---- | --------------------------------------------------------------------------------------------------------------- |
| 9:16 | stacked vertically: text from the top third to the middle, visual in the middle, CTA above the bottom safe zone |
| 4:5  | stacked but tighter: smaller visual, shorter gaps between blocks                                                |
| 1:1  | visual in the top half and text in the bottom half, or text over the visual with clear contrast                 |
| 16:9 | two columns: text on the left (about half the width at most), visual on the right                               |

- Motion direction may change so it feels natural (a wipe from below in 9:16 becomes a wipe from the left in 16:9), as long as the timing is the same.
- The text is the same in every size. If a line does not fit in 1:1, change the text box width or font size, not the sentence; rewrite only with the user's approval. The guide's word limit applies in every size.

## 4. Layout constants

One `LAYOUT` object holds every size-dependent number, picked by a key from `#root` and passed to CSS as variables. A file for another size only changes `data-width`, `data-height`, the `html`/`body` size and `<meta name="viewport">`; the rest is identical.

```css
/* Size-dependent numbers come from variables, never written here. Same for .visual and .cta. */
.title {
  position: absolute;
  left: var(--title-x);
  top: var(--title-y);
  width: var(--title-w);
  font-size: var(--title-size);
}
```

```js
const root = document.getElementById("root");
const [W, H] = [+root.dataset.width, +root.dataset.height];
const KEY = W > H ? "16:9" : W === H ? "1:1" : H / W > 1.5 ? "9:16" : "4:5";
// Canvas px, already inside each size's safe zone. `enter` is the entry offset.
const VARS = "title-x title-y title-w title-size visual-x visual-y visual-w cta-x cta-y".split(" ");
const LAYOUT = {
  "9:16": { css: [80, 280, 920, 180, 240, 700, 600, 80, 1340], enter: { x: 0, y: 140 } },
  "4:5": { css: [80, 100, 920, 160, 290, 480, 500, 80, 1060], enter: { x: 0, y: 120 } },
  "1:1": { css: [80, 610, 920, 120, 290, 90, 500, 80, 900], enter: { x: 0, y: 100 } },
  "16:9": { css: [120, 260, 840, 170, 1060, 170, 740, 120, 700], enter: { x: -160, y: 0 } },
};
const L = LAYOUT[KEY];
VARS.forEach((v, i) => root.style.setProperty(`--${v}`, L.css[i] + "px"));

// Scene times: IDENTICAL in every size file.
const T = { hook: 0, product: 1.6, price: 5.2, ending: 10.8, end: 15 };
const tl = gsap.timeline({ paused: true });
const settle = { x: 0, y: 0, opacity: 1, duration: 0.6, ease: "expo.out" };
tl.fromTo(".title", { ...L.enter, opacity: 0 }, settle, T.hook + 0.2);
// ... other scenes read T and L and never write their own pixel numbers
window.__timelines = window.__timelines || {};
window.__timelines["main"] = tl;
```

- These numbers are only a starting point; fit them to the scene content and check in the preview.
- Travel distance (`enter`) is layout too: 140 px on a 1920 px side feels like a shorter move on a 1080 px side.
- Random decoration: `x = rnd() * W`, `y = rnd() * H` with the same seeded PRNG, so the pattern follows the canvas.
- If a scene needs a truly different arrangement (two columns in 16:9), a small `if (KEY === "16:9")` branch is fine in the layout part, never in the timing part.

## 5. Steps

1. Finish the main size until it is approved.
2. Offer the extra sizes with the total credit they will use. Continue only if the user agrees.
3. Copy the HTML; change `data-width`, `data-height`, the `html`/`body` size and the viewport. Fill `LAYOUT` for that size. Do not touch `T`, the text or the tween order.
4. `preview_composition` per size, with a title naming the size, e.g. "Promo Es Kopi (4:5)".
5. The user presses "Buat video" per size (or `render_composition` per size in apps without a card).

## Check per size

- All text sits inside that size's safe zone; nothing clipped or overlapping.
- Timeline length and `data-duration` match the main size; scenes change at the same seconds.
- No pixel number left over from another size (e.g. `top: 1500px` on a 1080 px canvas).
- Visuals are not just shrunk: in 16:9 the main visual uses the screen width, not a narrow strip in the middle.

Further reading via `skill_library`: `motion-design/shot-composition/SKILL.md` (composition and grids per format), `hyperframes/hyperframes-core/SKILL.md` (the composition contract: `data-width`, `data-height`, duration).

## Adapted from

- [smwbev/framewright](https://github.com/smwbev/framewright) (MIT, © Evgenii (smwbev)), commit `7d3245a`: `.agents/skills/framewright/SKILL.md`, `.agents/skills/framewright/references/guide.md`, `.agents/skills/framewright/references/troubleshooting.md`, `.agents/skills/framewright/assets/skeleton.html`.

Changes by Mosio: translated and reduced to its principles: each frame as a pure function of time, seed and size; coordinates on a 1080 short side; layouts re-arranged per size instead of scaled. Dropped 2D canvas, Puppeteer, ffmpeg, synthetic voice, portraits from photos and checker scripts; added the per-size `LAYOUT` pattern with CSS variables, the re-layout table, and the rule to offer extra sizes first because each one is charged. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
