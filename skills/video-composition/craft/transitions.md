# Transitions

Seam recipes for Mosio's paused GSAP timeline: five velocity-matched cuts, two in-scene techniques, and the render prerequisites that prevent white flashes. The laws behind them (axis, direction, speed, cut mid-motion, one flow per video) are in `motion.md` section 3.

## Render prerequisites

- **Stage background (white-flash guard).** During a cut the two scenes' opacities can add up to less than 1 (cut-the-curve fades both, zoom-through dips to 0.15). Without a background the renderer shows white. Give `html, body` and `#root` a solid background (`background: var(--bg)`). If scenes have their own colors, put one darker than both on `#root`, or a solid `#stage` layer behind all scenes.
- **One wrapper per scene**: `<div class="scene" id="s2">` with `position: absolute; inset: 0`. Transitions move only the wrapper (`x`, `y`, `scale`, `opacity`/`autoAlpha`, `clipPath`, small blur). Content has its own entrance after the cut.
- **Hide later scenes from frame 0 through the timeline.** A plain `tl.set(el, {...}, 0)` does not render at frame 0, so add `immediateRender: true`. Entry tweens that start at the cut get `immediateRender: false`, so their start state does not leak early: `tl.set(".scene:not(#s1)", { autoAlpha: 0, immediateRender: true }, 0);`
- **One side per frame.** A cut is not a dissolve: on the cut frame hide the old scene (`tl.set(old, { autoAlpha: 0 }, CUT)`) while the new one is on.
- **Small blur (CPU render).** Text and headline groups 6-10 px, only for a few frames around the cut. Full-screen wrappers: preferably none (speed already sells it), at most 6-8 px for 0.3 s. Tween opacity separately from blur. Never `backdrop-filter`.
- **Budget.** 2-3 transition types per video, repeated. Default scene boundary: cut-the-curve along the flow.

| Technique            | For                                                   | Axis             | Total              | Ease                       |
| -------------------- | ----------------------------------------------------- | ---------------- | ------------------ | -------------------------- |
| Cut the curve        | ordinary scene boundary (default)                     | x or y           | ~0.6 s             | `power4.in` / `power4.out` |
| Zoom-through         | deeper into the same idea, headline swap              | scale, growing   | ~0.7 s             | `power3.in` / `expo.out`   |
| Inverse zoom-through | arrival: price, product, ending                       | scale, shrinking | ~0.7 s             | `power3.in` / `expo.out`   |
| Waterfall cut        | headline to headline, word by word                    | x, per word      | ~0.6 s             | `power4.in` / `power4.out` |
| Rack-focus blur-cut  | new state of the same surface, a cut meant to be seen | x, y or scale    | ~0.65 s            | `power2.in` / `power2.out` |
| Waterfall entry      | headline or list arriving inside a scene              | y from below     | 0.1-0.2 s per item | `power4.out`               |
| Nudge curve          | sliding a group to make room                          | x or y           | ~0.6 s             | three phases               |

**Partial travel:** about 12% of the frame side along the axis, never fully off-screen: 1920 px gives ~230 px, 1350 px ~160 px, 1080 px ~130 px.

## 1. Cut the curve (default)

A's hero accelerates one way, the cut lands mid-motion, B's hero continues the same way while slowing. Mirrored eases with equal distance and duration match the speed at the cut. A's opacity runs out exactly at the cut; B comes in at 0.35, already moving. Exit 0.2-0.4 s, entry the same or longer; optional blur 6-8 px.

```js
// dir -1 = left (or up when axis is "y"); travel about 12% of the frame side
function cutCurve(tl, a, b, CUT, { axis = "x", dir = -1, travel = 130, dur = 0.33 } = {}) {
  const d = travel * dir;
  tl.to(a, { [axis]: d, duration: dur, ease: "power4.in" }, CUT - dur);
  tl.to(a, { opacity: 0, duration: dur, ease: "power2.in" }, CUT - dur);
  tl.set(a, { autoAlpha: 0 }, CUT);
  const to = { [axis]: 0, autoAlpha: 1, duration: dur, ease: "power4.out" };
  tl.fromTo(b, { [axis]: -d, autoAlpha: 0.35 }, { ...to, immediateRender: false }, CUT);
}
cutCurve(tl, "#s1", "#s2", 2.8); // opener to product (2.8 s), flow to the left
```

For a light pull, add scale shrinking on both sides (A `1` to `0.92`, B `1.08` to `1`). Never pair a shrinking A with a B growing from small.

## 2. Zoom-through and 3. inverse zoom-through

Scale axis; the two texts are never visible together. Forward: everything grows, A `1` to `1.2`, B in from `0.75` to `1`. Inverse: everything shrinks, A `1` to `0.8`, B arrives oversized from `1.25` to `1`; keep it for arrivals. Tween the exit opacity linearly and separately (`power3.in` holds opacity too long). Use on headline groups or short phrases, not paragraphs or heavily blurred full-screen scenes.

```js
function zoomThrough(tl, a, b, CUT, { inverse = false, blur = 8 } = {}) {
  const [out, inn] = inverse ? [0.8, 1.25] : [1.2, 0.75];
  const bl = `blur(${blur}px)`;
  tl.to(a, { scale: out, filter: bl, duration: 0.2, ease: "power3.in" }, CUT - 0.2);
  tl.to(a, { opacity: 0.15, duration: 0.2, ease: "none" }, CUT - 0.2); // linear, separate
  tl.set(a, { autoAlpha: 0 }, CUT);
  const e = { duration: 0.5, ease: "expo.out", immediateRender: false };
  tl.fromTo(b, { scale: inn, filter: bl }, { scale: 1, filter: "blur(0px)", ...e }, CUT);
  tl.fromTo(b, { autoAlpha: 0.15 }, { autoAlpha: 1, ...e }, CUT);
}
```

Scale-sign discipline: for about 0.5 s after an inverse zoom, B's content arrives as a finished layout inside the shrinking wrapper. Do not pop its content in from small in that window; start entrances once the wrapper settles.

## 4. Waterfall cut

Cut-the-curve per word, for headline to headline. Old words peel off one by one, new words follow as an accelerating wave. Each beat stays within the guide's word limit. Words need `display: inline-block`. Exit: 0.34 s `power4.in` starting 0.32 s before the cut, 0.022 s stagger in reading order, with a 0.18 s `power1.in` fade starting with the x move. Entry: 0.3 s `power4.out` from opacity 0.35, first gap 0.05 s, times 0.84 per word. A short first beat may exit as one line, its fade ending about 0.02 s before the cut.

```js
const [w1, w2] = ["#k1 .w", "#k2 .w"].map((s) => gsap.utils.toArray(s));
const [C, T] = [4.6, 130]; // cut time, travel px
tl.set(w2, { x: T, autoAlpha: 0, immediateRender: true }, 0);
w1.forEach((el, i) => {
  const s = C - 0.32 + i * 0.022;
  tl.to(el, { x: -T, duration: 0.34, ease: "power4.in" }, s);
  tl.to(el, { opacity: 0, duration: 0.18, ease: "power1.in" }, s);
});
const to = { x: 0, autoAlpha: 1, duration: 0.3, ease: "power4.out", immediateRender: false };
let [off, gap] = [0, 0.05];
w2.forEach((el) => {
  tl.fromTo(el, { x: T, autoAlpha: 0.35 }, { ...to }, C + off); // fresh copy: fromTo writes into its vars
  [off, gap] = [off + gap, gap * 0.84];
});
```

## 5. Rack-focus blur-cut

The one cut meant to be seen: a blur spike hides a hard one-frame swap, like a focus pull. Use it for a new state of the same surface (regular menu to promo menu, app screen before and after). A stays opaque until the cut; cut at peak blur. At most once per ~8 s, never while text is being read. In Mosio the peak is 6-10 px (not 12-18 px), with a slight 1.06 scale as lens breathing.

```js
const T = 8.4; // A slides and blurs, staying opaque
tl.to("#s3", { x: -80, scale: 1.06, filter: "blur(8px)", duration: 0.3, ease: "power2.in" }, T);
tl.set("#s3", { autoAlpha: 0 }, T + 0.3);
const from = { autoAlpha: 1, x: 80, scale: 1.06, filter: "blur(8px)" };
const to = { x: 0, scale: 1, filter: "blur(0px)", duration: 0.35, ease: "power2.out" };
tl.fromTo("#s4", from, { ...to, immediateRender: false }, T + 0.3);
```

## 6. Waterfall entry (inside a scene)

Words or items snap in from below, each starting before the previous one rests. Binary opacity: `tl.set` to visible, then `to` the position, no fade. One direction per cascade, `power4.out` (never `inOut`). Heavy items travel farther and longer, light ones flick: anchors (key word, price) 60-80 px over 0.16-0.20 s with a 0-2 frame gap; ordinary words 40-50 px, 0.13-0.16 s, 1 frame overlap; light marks (punctuation, units) 30-48 px, 0.10-0.13 s, 1-2 frames overlap.

```js
const F = 1 / 30; // one frame
const items = [
  ["#j1", 80, 0.18, -2],
  ["#j2", 45, 0.15, 1],
  ["#j3", 40, 0.14, 1],
  ["#dot", 48, 0.12, 2],
];
const sels = items.map((e) => e[0]);
tl.set(sels, { autoAlpha: 0, immediateRender: true }, 0);
let t = 3.1; // cascade start
items.forEach(([sel, y, dur, overlap]) => {
  tl.set(sel, { autoAlpha: 1, y }, t);
  tl.to(sel, { y: 0, duration: dur, ease: "power4.out" }, t);
  t += dur - overlap * F; // negative overlap = gap
});
```

The last word may split into two parts that travel farther to stretch the climax. Once settled, the group usually slides to make room: the nudge curve.

## 7. Nudge curve (inside a scene)

Slide an arranged group (a row of words, a stack of menu cards) slow, fast, slow. A stock `power4.inOut` stops with a jolt; chain three tweens on the same property: wind-up `power3.in` (~10% distance, ~20% time), surge `none` (~65%, ~18%), tail `power4.out` (~25%, ~62%).

```js
const t0 = 5.2; // slide left 270 px in 0.57 s
tl.to("#row", { x: -30, duration: 0.12, ease: "power3.in" }, t0)
  .to("#row", { x: -210, duration: 0.1, ease: "none" }, t0 + 0.12)
  .to("#row", { x: -270, duration: 0.35, ease: "power4.out" }, t0 + 0.22);
```

The tail is at least 3x the wind-up; if it still jolts, lengthen the tail time, not its distance. Bring new content in during the surge. For other distances, scale the distances and keep the time ratios.

**Other catalogs.** Curtain and iris wipes (`clipPath`) are in `motion.md`. Full catalog via `skill_library`: `hyperframes/hyperframes-animation/transitions/overview.md` (pick by energy and mood), `transitions/catalog.md`, `transitions/css-push.md`, `css-radial.md`, `css-scale.md`, `css-cover.md`, `css-grid.md`; velocity matching in `hyperframes/hyperframes-animation/techniques.md` section 10, `rules/waterfall-entry.md` and `rules/nudge-curve.md`. Ready-made transitions: `search_registry` ("transition", "wipe", "zoom"). When using them in Mosio: skip shader or WebGL transitions, ignore their font hints and per-clip `data-start`, hide scenes through the timeline (not `opacity: 0` in CSS), and lower blur to the limits above.

**Before `preview_composition`:** solid background on `html`, `body` and `#root`; later scenes hidden at frame 0; each cut keeps axis and direction with mirrored eases and both sides moving; no unintended overlap of two scenes or headlines, no text clipped by a wipe; blur within the limits above.

## Adapted from

- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `.claude/skills/cut-the-curve/SKILL.md`, `.claude/skills/cut-the-curve/examples/gsap-implementation.md`, `.claude/skills/seam-craft/SKILL.md`.

Changes by Mosio: translated, condensed and rewritten for one paused Mosio GSAP timeline (no audio, no CDN): registry template and injector code (`__OLD__`, `data-start`, ping-pong tracks) became small functions on the same timeline, initial states moved from `gsap.set` and CSS to `tl.set` with `immediateRender: true`, and blur was lowered for CPU render. Dropped the Seam Gate scripts, JSON ledger and CLI commands; added travel distances per Mosio size, small-business examples and notes on using the vendored catalog. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
