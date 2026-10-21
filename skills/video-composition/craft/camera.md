# Camera: one 2D world

Use when the video lives in one illustrated world and a camera moves through it: the `visual-story` type (`../../video/types/visual-story.md`), and any explainer that zooms into a detail. Everything is 2D SVG on the one paused timeline; true 3D (three.js, WebGL) is not available in Mosio today, so depth is faked (section 6). Event times come from the caption plan's `at()` (`captions.md`). Full example: the `visual-story` style.

## 1. One world, one `render(t)`

- One inline SVG (1080 × 1920) with layers: `#bokeh`, `#far`, `#world` (the subject), `#near` (a blurred foreground object). Captions, tags and counters are HTML outside the camera, so read text holds still.
- Camera keys are a focus point in world pixels and a scale. One `render(t)` computes every layer and writes its `transform` attribute. A proxy tween drives it, like the guide's counters.
- Do not use GSAP `svgOrigin` or `transformOrigin` on nested SVG groups: unreliable. Positions, scales, flights and characters all come from `render(t)`; GSAP tweens only opacity, fills and simple one-off pops.

```js
const CY = 1000; // screen y the focus lands on (the hero zone)
const io = (u) => (u < 0.5 ? 4 * u * u * u : 1 - Math.pow(-2 * u + 2, 3) / 2);
const lerp = (a, b, u) => a + (b - a) * u;
const clamp01 = (v) => Math.max(0, Math.min(1, v));
const T = at("segi\u00a0enam?");
const CAMK = [
  { t: 0, f: [540, 1000], s: 1 },
  { t: T - 0.4, f: [540, 1000], s: 1.07, lin: true }, // always alive: slow drift
  { t: T, f: [610, 930], s: 2.3 }, // push-in lands on the word
  { t: T + 0.3, f: [610, 930], s: 2.36, lin: true }, // settle
];
function cam(t) {
  let i = 0;
  while (i < CAMK.length - 1 && t >= CAMK[i + 1].t) i++;
  const [a, b] = [CAMK[i], CAMK[Math.min(i + 1, CAMK.length - 1)]];
  const r = b.t > a.t ? clamp01((t - a.t) / (b.t - a.t)) : 1;
  const u = b.lin ? r : io(r);
  return { fx: lerp(a.f[0], b.f[0], u), fy: lerp(a.f[1], b.f[1], u), s: lerp(a.s, b.s, u) };
}
// a layer with parallax factor k follows the camera k of the way
const camT = (c, k = 1) => {
  const s = 1 + (c.s - 1) * k;
  const [fx, fy] = [lerp(540, c.fx, k), lerp(CY, c.fy, k)];
  return `translate(${(540 - fx * s).toFixed(2)} ${(CY - fy * s).toFixed(2)}) scale(${s.toFixed(4)})`;
};
const PARALLAX = { bokeh: 0.1, far: 0.45, world: 1, near: 1.4 };
const LAYERS = Object.entries(PARALLAX).map(([id, k]) => [document.getElementById(id), k]);
function render(t) {
  const c = cam(t);
  for (const [el, k] of LAYERS) el.setAttribute("transform", camT(c, k));
  // idle motion from t (wing flap, bob, drift), flights, pops: all computed here
}
const clock = { t: 0 }; // END: the composition length, as in data-duration
tl.to(clock, { t: END, duration: END, ease: "none", onUpdate: () => render(clock.t) }, 0);
render(0);
```

A world point (x, y) lands on screen at `540 + (x - fx) * s`, `CY + (y - fy) * s`. Idle motion (`ry` of a wing from `Math.sin(t * 40)`) also lives in `render(t)`, so it needs no repeating tween. Particles: seeded mulberry32, positions from `t`, 60 or fewer.

## 2. Moves

| Move      | How                                                                 | Lands on                          |
| --------- | ------------------------------------------------------------------- | --------------------------------- |
| Push-in   | scale about the focus onto the element the sentence is about        | the word that names it            |
| Pull-back | the reverse; the detail becomes the system                          | the word that changes scale       |
| Pan       | move the focus between parts of the same world instead of cutting   | the word that names the next part |
| Drift     | a `lin` key, 2-7% scale over a beat; the camera is never dead still | between moves                     |

- Big moves take 0.2-0.4 s (up to 0.7 s for long pulls), ease in-out, and **arrive on the token time**, then a 0.3 s settle (a `lin` key, 2-3% more scale).
- Keep the subject in the hero zone (y 620-1300): portrait frames are narrow, so pull back further than feels natural and keep the bottom safe zone free of anything that matters.

## 3. Shared-element hand-off

The element that matters flies into its role in the next picture (0.3-0.6 s, `io`, a slight arc) while the rest fades behind it; the flier stays sharp.

- At the hand-off frame the flying copy must be **pixel-identical** to the original. Draw the copy with the same art at its local origin; compute its start with `cam(TH)` (a pure function, safe at build time): position `540 + (x - fx) * s`, `CY + (y - fy) * s`, scale `s` times the art's own scale.
- On that same frame: `tl.set(original, { opacity: 0 }, TH)` and `tl.set(copy, { opacity: 1 }, TH)`.
- Positions come from your geometry data. **Never measure the DOM** (`getBBox`, `getBoundingClientRect`) during a seek.
- In `render(t)`: `u = io(clamp01((t - TH) / 0.5))`, position lerped to the target with `- 40 * Math.sin(Math.PI * u)` on y for the arc, scale lerped to the target scale.
- The outgoing world fades out in 0.3 s (`power2.in`); a blur of 8 px or less only on a small group, never full-screen (CPU).

## 4. Breakdowns: open the object on screen

- **Exploded view**: parts fly out along the line from the object's centre, staggered by distance from the centre (about 0.12 s per unit), then keep floating (a small `sin` drift from `t`).
- **Explode-into-measure (2D bars)**: build each shape from separate edge `<line>`s, scaled to area 1 so each perimeter is the comparable number (triangle 4,56, square 4,00, hexagon 3,72). Each edge keeps its length, moves its midpoint to its slot in a row (end to end, one row per shape, a small upward arc) and rotates to the nearest flat angle; stagger 0.04 s by index. Bars tint to their semantic colours; the counter sits at the computed bar end and counts from the token. Reassemble by running the same parameter back.
- **X-ray**: crossfade the shell to about 15% opacity, show the inside, then draw thin leader lines to labels (`DrawSVGPlugin` or `stroke-dashoffset`).
- **Cutaway**: a `clipPath` rect over the shell slides open, so inside and outside show together.
- **Medium as particles**: invisible stuff (air, data, honey) as particles whose density or colour carries the meaning.

## 5. Text as an object

- Concept words can live in the scene (Playfair Display italic from `vendor/`), against the sans captions. Scene labels enter with the caption blur-in (`captions.md`).
- **Per-glyph morph** (one word becomes another): one element per letter, x from a per-letter width table you write (no measuring; confirm in the preview). Reused letters glide to their new x, unused ones fall with a little rotation and fade, new ones blur in. About 0.4 s in total, 15-25 ms stagger.

## 6. Depth in 2D

- **Parallax**: bokeh 0.1×, far layer 0.45×, subject 1×, a blurred near object 1.3-1.6×.
- **Softness**: far and near layers get one static blur filter (never animated), or use pre-blurred shapes (radial gradients for bokeh), cheaper on the CPU.
- **Shadows and light**: a soft ellipse under each object, a specular highlight on liquids, a vignette `radial-gradient` on top.
- **Isometric shading**: top face base colour, left face `color-mix(in srgb, base 80%, black)`, right face the same at 60%.
- **Flat to isometric** (stands in for the 2D-to-3D morph): interpolate the stage group's matrix from the top view `matrix(1 0 0 1 0 0)` to `matrix(0.866 0.5 -0.866 0.5 0 0)` about the stage centre, then extrude walls as side-face polygons from the projected top edge up by `h` (0 to the wall height), drawn back to front. The drawing "stands up". Reverse it to return to the flat illustration.
- **Entering depth, the usual order** (a strong default, not a rule): tilt the flat drawing that is already on screen, let its walls stand up, then lift the element that matters out of it (a shared element that starts on the same pixels) and give it its new role, e.g. a tall cell filling with honey for "madu sama". Viewers see the same thing gain a dimension, so the idea carries over. Skip it when it does not serve the beat: a quick aside, a new world after a flash or fly-through, a film that never leaves 2D, or when the timing leaves no room (a tilt needs about 0.6-1 s). What to avoid is the accidental version: flat shapes on one frame and unrelated 3D-looking objects popping in on the next.

## 7. Fly-through

- Dive the camera into a surface: scale up hard with an ease-in (to 8-12× over about 0.5 s).
- A full-frame radial colour flash (gold for honey) fades in over the last 0.15 s.
- While the flash is fully up, cut the world (`tl.set` on opacity) to the interior set (honey-amber background, rising bubbles), then fade the flash out over 0.2 s.
- Back out the same way: flash, cut, a fast ease-out pull-back to a wide view.
- Declare `CUT_IN` and `CUT_OUT` once and use them in both `render(t)` and the timeline.

**Before `preview_composition`:** every layer's transform comes from `render(t)`; `render(0)` runs at build; no DOM measuring; each move arrives on its token; hand-offs show no jump; the subject stays in the hero zone and out of the caption band.

## Adapted from

Original to Mosio: adapted from the team's own visual-story skill (the bees and hexagon explainer). Changes by Mosio: made silent (captions carry the story, timed at reading pace, no voice-over or TTS), 2D only (no three.js or WebGL), 30 fps, and fitted to Mosio's composition rules and tools; the Python, CLI and voice pipeline was dropped.
