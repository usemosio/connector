# Photo slideshow

Turns 3-5 product photos (clothes, cakes, crafts, a boarding room, finished service work) into a video that feels filmed, not pasted: each photo moves gently in its own way, gets a name and price, and the video ends on a CTA.

## Size budget: read this first

Photos are allowed only as `data:` images inside the composition, and the whole composition is at most 200,000 characters. Base64 adds about a third to the file size:

| Photos | Size per photo | Approx. image characters |
| ------ | -------------- | ------------------------ |
| 3      | max 35 KB      | about 145,000            |
| 4      | max 30 KB      | about 165,000            |
| 5      | max 25 KB      | about 172,000            |

- Leave 12,000-20,000 characters for HTML, CSS and script. JPEG or WebP, 720 px wide (WebP is sharper at the same size), quality 55-70. Each photo appears **once**, in `<img src="data:image/webp;base64,...">`. Never reuse it (e.g. as a blurred backdrop): the size doubles.

## Handling the photos

- Ask for the best 3-5 photos, one product or one angle each. **With a code tool** (Python sandbox or similar): shrink them there as preparation, not as part of the render, e.g. Pillow `im.thumbnail((720, 1280)); im.save(buf, "WEBP", quality=62)`. Check the base64 length; lower quality or size until it fits. Use the user's real photos unaltered.
- **Without a code tool** (plain chat): the assistant cannot shrink photos or write base64 reliably. Never type base64 by hand. Offer instead a fill-in recipe, which needs no photos (`list_recipes` with `kind: "fill_in"` or the `show_recipes` gallery, then `preview_recipe`), or an illustrated product card in SVG/CSS (shapes, brand colours, name, price) that clearly reads as an illustration, never as a product photo. Mosio makes and edits no images with AI models. Never promise "enhanced photos" or automatic background removal.

## One rule: no two photos move alike

Identical slow zoom-ins are the mark of an amateur slideshow. Vary direction, zoom in or out, and amount, but **deterministically**: seeded mulberry32, never `Math.random()`, so preview and render match.

| Knob               | Range                                          | Note                                                       |
| ------------------ | ---------------------------------------------- | ---------------------------------------------------------- |
| Zoom               | in 1.08 to 1.12-1.15, or out the reverse       | above 1.15 feels like a crash zoom                         |
| Pan                | left, right, up, down or diagonal, 16-40 px    | direction from the seed                                    |
| Ease               | `sine.inOut`                                   | Ken Burns without a jolt                                   |
| Origin             | `transform-origin` toward the subject          | product or face stays framed                               |
| Safe pan (`cover`) | at most `(smallest scale - 1) × box width / 2` | 1080 px box at 1.08: about 43 px, else an empty edge shows |

## Mixed aspect ratios: never stretch, compare photo and box ratio

| Strategy                     | When                                                                        | How                                                                                                 |
| ---------------------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Cover                        | ratios differ by less than about 15%                                        | `object-fit: cover`, Ken Burns on the `img`, `overflow: hidden` on the box                          |
| Contain on a colour backdrop | landscape photo in 9:16, or photo with text (pack label, menu, certificate) | whole photo as a card with a soft shadow over a gradient of its dominant colour or the brand colour |

- `contain` backdrop: a two-colour `color-mix` gradient, slow floating shapes or a line pattern. **Not** a blurred copy of the photo: heavy on the CPU render and it eats the character budget.
- For `contain`, move the card (scale 1.00 to 1.06, pan 10-24 px); no edge can leak. Portrait photos in 9:16 almost always suit `cover`. Landscape ones go `contain` in the upper third to middle, caption below.

## Caption and price

- Per photo: product name (2-4 words) plus a price or one benefit, at most 7 words. Prices only from the user; ask.
- Same position on every photo, e.g. y 1250-1480 in 9:16, clear of the guide's safe zones. Name 90-130 px, text 54-80 px, on a colour band or card so it reads over any photo.
- Captions stay still while the photo moves. In 0.3 s after the photo shows (masked `yPercent: 110` reveal), out 0.3 s before it changes. A price may count up per `mosio/video-composition/craft/numbers.md`, landing exactly on the real price.

## Silent pacing

| Part         | Time                                 | Note                                                                                           |
| ------------ | ------------------------------------ | ---------------------------------------------------------------------------------------------- |
| Opening      | 1-1.5 s                              | one short line or the shop name, big move                                                      |
| Per photo    | 2.5-4 s                              | under 2.5 s nobody sees it                                                                     |
| Hero photo   | 1.3-1.5x longer                      | a rhythm, not a metronome                                                                      |
| Transition   | 0.4-0.6 s overlap                    | one kind only: crossfade or a one-way `clipPath` wipe (mixing wipes, spins, cubes looks cheap) |
| Ending       | 2 s+ held                            | CTA, name, real contact                                                                        |
| 15 s example | 1.2 + (3.0 + 3.0 + 4.2 + 2.6) + 2 s+ | opening, four photos, ending                                                                   |

Usual roles: hook → offer → offer → offer → cta, one `offer` per photo; the hero photo gets weight `long`.

## Core code

Each photo is a `.slide` (absolute, full screen, in order so the next stacks on top) holding an `img` and a `.cap` with masked spans.

```js
const mulberry32 = (a) => () => {
  a |= 0;
  a = (a + 0x6d2b79f5) | 0;
  let t = Math.imul(a ^ (a >>> 15), 1 | a);
  t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
  return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
};
const rnd = mulberry32(20241);
// [pan x, pan y, zoom], shuffled once by the seed so every photo differs
const MOVES = [
  [1, 0, "in"],
  [-1, 0, "in"],
  [0, 1, "in"],
  [0, -1, "in"],
  [1, 1, "out"],
  [-1, -1, "out"],
  [1, -1, "out"],
  [-1, 1, "in"],
]
  .map((g) => [rnd(), g])
  .sort((a, b) => a[0] - b[0])
  .map((p) => p[1]);
const SHOW = [3.0, 3.0, 4.2, 2.6],
  CROSS = 0.5,
  tl = gsap.timeline({ paused: true });
let t = 1.2;
document.querySelectorAll(".slide").forEach((s, i) => {
  const [gx, gy, dir] = MOVES[i],
    dz = 0.04 + rnd() * 0.03,
    dp = 16 + rnd() * 24;
  const [s0, s1] = dir === "in" ? [1.08, 1.08 + dz] : [1.08 + dz, 1.08];
  const cap = s.querySelectorAll(".cap span");
  tl.fromTo(s, { opacity: 0 }, { opacity: 1, duration: CROSS, ease: "power1.inOut" }, t)
    .fromTo(
      s.querySelector("img"),
      { scale: s0, x: 0, y: 0 },
      { scale: s1, x: gx * dp, y: gy * dp, duration: SHOW[i] + CROSS, ease: "sine.inOut" },
      t,
    )
    .fromTo(
      cap,
      { yPercent: 110 },
      { yPercent: 0, duration: 0.6, ease: "expo.out", stagger: 0.08 },
      t + CROSS + 0.3,
    )
    .to(cap, { yPercent: -110, duration: 0.3, ease: "power2.in" }, t + SHOW[i] - 0.3);
  t += SHOW[i];
});
// t = ending start. data-duration = t + ending (at least 2.5).
```

- The photo moves through its show time plus the transition, so it never freezes mid-crossfade. `contain` photos: same tween, smaller values, on the card, not the `img`.
- Eight moves cover 3-5 photos without repeats; change the seed if a result looks off, then lock it. Per-photo `transform-origin` goes in CSS (e.g. `50% 35%` when the product sits high); it is not a starting transform.

## Opening, ending, other sizes

- Opening: shop name or a short promise ("Koleksi Lebaran baru") with a masked reveal, or the first photo enters on a slightly faster zoom-out (1.2 s). Ending: the last photo dims or wipes to the brand colour; CTA, name and contact hold 2 s+; 0.5 s fade. For the ending and price, borrow from the `product-hero` and `menu-board` recipes.
- 4:5 and 1:1: wider box, so more landscape photos suit `cover`; caption on a bottom band. 16:9: photo on the left two thirds, name and price on the right third, so portrait photos show whole. See `mosio/video-composition/craft/sizes.md`.

## Check before sending

- Whole HTML under 200,000 characters; each photo appears once. No two photos share a move (seeded PRNG); no stretched photo, no empty edge while panning.
- Captions fixed, inside the safe zones, at most 7 words; prices from the user. Ending holds 2 s+ and the timeline length equals `data-duration`.

## Adapted from

- [iart-ai/ecommerce-video-skills](https://github.com/iart-ai/ecommerce-video-skills) (MIT, © iart.ai), commit `2bd0dd1`: `skills/photo-slideshow/SKILL.md`, `skills/photo-slideshow/references/ken-burns.md`, `skills/photo-slideshow/references/aspect-and-layout.md`, `skills/photo-slideshow/references/timing-and-audio.md`, `skills/photo-slideshow/references/templating-folder.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/slideshow/SKILL.md` (one idea per slide, minimum text sizes, `contain` for photos with text). Mosio's copy: `hyperframes/slideshow/SKILL.md` via `skill_library`; that skill is for interactive presentation decks, not video.

Changes by Mosio: condensed and rewritten from Remotion to one paused GSAP timeline (Ken Burns as tweens, seeded mulberry32, no audio, no CDN). Music beat sync, librosa beat detection, blurred photo backdrops, folder manifests and EXIF, ffmpeg rendering and interactive decks were dropped; added the 200,000-character budget for `data:` photos, handling with and without a code tool, colour backdrops for `contain`, the safe pan limit, per-photo captions and prices, and silent pacing. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
