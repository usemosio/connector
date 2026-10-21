# Logo and end card

Use to bring in a business logo: as a short sting inside a video, as a standalone logo video, or as the end card (logo, business name, CTA, contact). The guide's rules still apply: the ending holds at least 2 s and contacts are never invented.

## 1. Ask first: what form is the logo in?

Small businesses often have only a PNG from Canva, a photo of a banner, or no logo at all. The technique depends on it:

| Asset                          | Fitting technique                                            | Avoid                                    |
| ------------------------------ | ------------------------------------------------------------ | ---------------------------------------- |
| Line SVG (monogram, line icon) | path draw, then fill                                         | complex morphs                           |
| Filled SVG, several parts      | assemble part by part, or mask wipe                          | drawing strokes on shapes that have none |
| PNG or WebP                    | mask reveal (circle, wipe), slight scale down, a light sweep | path draw (raster has no lines)          |
| No file                        | text wordmark: the business name in one font, one accent     | redrawing the logo as if official        |

- The image goes in as `data:` inside the HTML. Shrink it first: about 400-600 px wide, transparent background, ideally under 80,000 base64 characters, so the composition stays under the guide's limit. Never show it larger than its real size.
- If the image cannot be turned into `data:` in this conversation, use a text wordmark and say so plainly. To imitate the logo shape with simple SVG, ask the user first and call it a simplified version, not the official file.

## 2. One idea per logo

Pick ONE technique: draw, assemble or mask. Not all at once.

- Brand geometry stays intact: uniform scale, move, rotate, opacity and masks are safe. Avoid `scaleX` different from `scaleY`, skew and squash on the final logo (a bounce under 0.2 s that returns to 1 is fine). Brand colors do not change mid-reveal.
- Keep clear space around the logo (about the cap height of the business name) free of passing particles or lines.
- Lockup order: symbol, business name, tagline, contact last. The eye ends on the name.

| Use                                      | Total                 | Reveal        | Hold                    |
| ---------------------------------------- | --------------------- | ------------- | ----------------------- |
| Sting inside a video (opener or divider) | 1.5-3 s               | 0.8-1.5 s     | the rest                |
| Standalone logo video                    | 3-5 s                 | at most 2.5 s | at least 1 s            |
| End card with contact                    | 3-4 s in a 15 s video | 1-1.5 s       | at least 2 s (required) |

Simple logos sit near 0.8-1.4 s, long wordmarks near 2-2.5 s. After the hold, finish as the guide says (fade or scale out in the last 0.5 s).

## 3. Path draw (SVG)

The stroke becomes one long dash whose offset is pulled to 0. `pathLength="1"` lets every path use the value 1 without measuring. `DrawSVGPlugin` (`vendor/DrawSVGPlugin.min.js`) also works.

```html
<svg id="mark" viewBox="0 0 200 200">
  <path class="ink" pathLength="1" d="M60 40 L60 160" />
  <path class="ink" pathLength="1" d="M140 40 L140 160" />
  <path class="ink" pathLength="1" d="M60 100 L140 100" />
</svg>
```

```css
#mark .ink {
  fill: none;
  stroke: var(--main);
  stroke-width: 12;
  stroke-linecap: round;
  stroke-linejoin: round;
}
```

```js
const tl = gsap.timeline({ paused: true });
const draw = { attr: { "stroke-dashoffset": 0 }, duration: 0.6, ease: "power2.out" };
gsap.utils.toArray("#mark .ink").forEach((p, i) => {
  const from = { attr: { "stroke-dasharray": 1, "stroke-dashoffset": 1 } };
  tl.fromTo(p, from, { ...draw }, 0.3 + i * 0.45); // next stroke starts at ~75% of the previous
});
const rise = { yPercent: 0, duration: 0.5, ease: "expo.out", stagger: 0.04 };
tl.fromTo("#name .s", { yPercent: 110 }, rise, 1.7);
```

- Stroke ease `power2.out` (hand lifts at the end) or `none` (even pen). Pens do not bounce: no `back.out` or `elastic`. 0.3-0.8 s per segment; over 1 s feels slow.
- Draw then fill: after the strokes, tween `fillOpacity` 0 to 1 (set `fill` in CSS, `fill-opacity: 0` through `fromTo`).
- The business name follows about 0.2 s after the last stroke.

## 4. Assemble lockup

For an SVG logo with parts (`<g class="part">`): base or container first, then accents, then the name.

```js
const pop = { scale: 1, opacity: 1, ease: "back.out(1.6)", transformOrigin: "50% 50%" };
tl.fromTo("#base", { scale: 0.8, opacity: 0 }, { ...pop, duration: 0.5 }, 0.3);
tl.fromTo(".accent", { scale: 0.7, opacity: 0 }, { ...pop, duration: 0.45, stagger: 0.08 }, 0.6);
const nameUp = { yPercent: 0, duration: 0.4, ease: "power3.out", stagger: 0.04 };
tl.fromTo("#name .s", { yPercent: 120 }, nameUp, 0.95);
tl.to("#logo", { scale: 1.04, duration: 0.12, ease: "power2.out", yoyo: true, repeat: 1 }, 1.4);
```

Variants from the `logo-assemble-lockup` blueprint that suit small businesses:

- **Clean text, logo blooms**: the tagline shows, then leaves; on the empty screen the logo grows from small with a bounce, slides left while the name opens on its right.
- **Underline sweep**: the lockup is centered from the start, an accent line sweeps under the name, the tagline wipes in left to right.
- **Assemble from words**: opening words (`kinetic-type.md`) end and the logo lands in the same spot.

## 5. Masks for PNG

Raster images are revealed with masks and scale, never drawn.

```js
// <img id="logo" src="data:image/png;base64,..." alt="" />
const reveal = { clipPath: "circle(71% at 50% 50%)", scale: 1, duration: 0.8, ease: "expo.out" };
tl.fromTo("#logo", { clipPath: "circle(0% at 50% 50%)", scale: 1.12 }, reveal, 0.3);
```

- Other directions: wipe left to right with `inset(0% 100% 0% 0%)` to `inset(0% 0% 0% 0%)`, or from below with `inset(100% 0% 0% 0%)`.
- Light sweep: one slanted transparent white gradient band moved by `xPercent` across a card or pill behind the logo, once.
- Soft glow behind the logo: a slowly growing radial gradient, not a large `filter: blur`.

## 6. Text wordmark (no logo file)

Treat the name as the logo: one font, one weight, one accent color.

- Neat letter drop: split into masked letters (the `splitWords` helper in `kinetic-type.md`, letter mode), stagger 0.03-0.05 s.
- Tracking in: `letterSpacing` from wide (0.4em) to normal while opacity rises, 0.8 s `expo.out`.
- Accent last: a dot, underline or accent-colored circle lands last, e.g. the dot in "Kopi Senja." or the line under "Laundry Kilat".
- A simple monogram (initials in a circle or rounded square) is fine if the user agrees and knows it is not an official logo.

## 7. End card

Top to bottom: logo or wordmark, business name, short CTA, contact.

- Contacts come only from the user: WhatsApp number, Instagram/TikTok handle, address, or Shopee/Tokopedia store name. If missing, ask; never put a sample number in a final video. Pick one main contact; the guide's word limit includes it.
- Enter in order: logo, name (0.2-0.3 s later), CTA, contact. Then everything holds at least 2 s, inside the guide's safe zones for the size used.
- During the hold one small thing may live: a CTA button breathing (`scale` 1 to 1.05 with `yoyo` and a fixed `repeat` count) or slow background light. The logo and contact text stay still.

**Before `preview_composition`:** the final frame shows the whole, clean logo (no half-drawn strokes, no mask still cutting); proportions and colors match the original; the `data:` image is sensibly small; the end-card contact came from the user and holds, readable, for at least 2 s.

Further reading via `skill_library`: `hyperframes/hyperframes-animation/blueprints/logo-assemble-lockup.md` (skip its 3D camera and heavy motion blur for CPU render); `hyperframes/hyperframes-animation/rules/svg-path-draw.md` (stroke durations and staggers); `motion-design/logo-animation/SKILL.md` and `references/logo-patterns.md` (technique table, brand-geometry rules; their Lottie, CSS @keyframes and sound-sync parts do not apply in Mosio). Registry (`search_registry`): "logo sting", "logo brand close", "cta lockup", "wordmark tiles", "svg stroke trace", "svg mask reveal", "light sweep pass".

## Adapted from

- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes-animation/blueprints/logo-assemble-lockup.md`, `skills/hyperframes-animation/rules/svg-path-draw.md`, `skills/motion-graphics/categories/logo-reveal/module.md`.
- [iart-ai/motion-design-skills](https://github.com/iart-ai/motion-design-skills) (MIT, © iart.ai), commit `3c129f7`: `skills/logo-animation/SKILL.md`, `skills/logo-animation/references/logo-patterns.md`.

Changes by Mosio: translated, condensed and adapted to Mosio's composition rules (one paused GSAP timeline, no audio, no CDN). Dropped Lottie, After Effects, sonic logo sync, endless loader loops, morphs with Flubber and `requestAnimationFrame`, 3D camera and Playwright checks; added guidance for PNG logos and businesses with no logo file, the `data:` size budget, durations per use, and end cards with real contacts. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
