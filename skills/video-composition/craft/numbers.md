# Numbers and charts

Use when a number is the proof: prices, customer counts, satisfaction rates, revenue, ad results. Every value is computed from timeline time, as the guide requires. Full examples: the `angka-capaian` style (big number, bar chart, percent ring) and `dasbor-iklan` (a metric falling then rising, a line chart drawing itself).

## 1. Honest numbers (required)

- **Ask for the numbers**: title, labels, exact values, period, and whether they are real or illustrative. Never invent figures, ratings, customer counts or percentages.
- **Label sample data** on every frame where it appears, e.g. "Contoh ilustrasi". Remove the label only when the user gives real figures.
- **Never change a value.** Formatting is fine (thousands dots, Rp, %); rounding or adding "+" is not, unless the user asks.
- **Honest scale.** Bars start at zero, heights are proportional, no cut axis to inflate a gap.
- **Show period and source** when known ("Okt 2026", "data toko"), small under the number.

## 2. id-ID formatting

Compute the raw value, round it, then format. Never let a fraction like `12873,4` show.

```js
const rp = (n) => "Rp" + Math.round(n).toLocaleString("id-ID"); // Rp18.000
const thousands = (n) => Math.round(n).toLocaleString("id-ID"); // 12.000
const pct = (n, d = 0) =>
  n.toLocaleString("id-ID", { minimumFractionDigits: d, maximumFractionDigits: d }) + "%"; // 98,5%
const times = (n) =>
  n.toLocaleString("id-ID", { minimumFractionDigits: 1, maximumFractionDigits: 1 }) + "x"; // 4,6x
// Short forms: rb (ribu), jt (juta), M (miliar). One decimal, comma separator.
const short = (n) => {
  const [b, s] =
    n >= 1e9 ? [1e9, " M"] : n >= 1e6 ? [1e6, " jt"] : n >= 1e3 ? [1e3, " rb"] : [1, ""];
  return (n / b).toLocaleString("id-ID", { maximumFractionDigits: 1 }) + s; // 48,9 jt
};
```

- "Rp" touches the number with no space: Rp18.000, Rp1,2 jt. Decimal comma, thousands dot.
- Keep digit width fixed so the layout does not shake: `font-variant-numeric: tabular-nums` if the font supports it, or a fixed-width box (`min-width` in `ch`, right or center aligned).

## 3. Count-up

Tween a proxy `{ v }` with an `onUpdate` that writes the text. A slowing ease (`power3.out`, `expo.out`) feels like landing on the number; `ease: "none"` only for evenly running values (a clock, years).

```js
const tl = gsap.timeline({ paused: true });
const TARGET = 18750; // the user's price, exact
const el = document.getElementById("price");
const o = { v: 0 };
// Steps of 50 stop digit flicker, but the last frame always shows the exact value.
const write = () => (el.textContent = rp(o.v >= TARGET ? TARGET : Math.round(o.v / 50) * 50));
tl.fromTo(el, { y: 60, opacity: 0 }, { y: 0, opacity: 1, duration: 0.5, ease: "expo.out" }, 0.4);
tl.to(o, { v: TARGET, duration: 1.4, ease: "power3.out", onUpdate: write }, 0.4);
```

- If you round in steps while counting, the final frame must write the exact value (as above). Rounding to 500 on a target of 18,750 would show Rp19.000: wrong.
- Count for 1.2-2 s, then hold. The meaning label ("pelanggan puas") appears after the number lands: value first, then meaning.
- The number may scale up slightly while counting (0.9 to 1) for weight; never from zero. One number moves at a time.

## 4. Hero stat with a ring

A lone number floats; pair it with a visual of its value (ring, fill bar, shape). Ring and number finish together on the same ease.

```html
<svg class="ring" viewBox="-50 -50 100 100">
  <g fill="none" stroke-width="10" transform="rotate(-90)">
    <circle r="42" stroke="rgba(255,255,255,0.15)" />
    <circle id="arc" r="42" stroke="#a3e635" stroke-linecap="round" pathLength="100" />
  </g>
</svg>
```

```js
const PCT = 87; // the user's figure
const p = { v: 0 };
const num = document.getElementById("pct");
const showPct = () => (num.textContent = pct(p.v >= PCT ? PCT : Math.round(p.v)));
const timing = { duration: 1.4, ease: "power2.out" }; // shared, so ring and number finish together
const arcFrom = { attr: { "stroke-dasharray": 100, "stroke-dashoffset": 100 } };
tl.fromTo("#arc", arcFrom, { attr: { "stroke-dashoffset": 100 - PCT }, ...timing }, 1.0);
tl.to(p, { v: PCT, ...timing, onUpdate: showPct }, 1.0);
```

`pathLength="100"` makes the offset read directly as percent; the `viewBox` centers the circles at 0 and `rotate(-90)` starts the arc at 12 o'clock. `DrawSVGPlugin` (`vendor/DrawSVGPlugin.min.js`) also works.

## 5. Bar chart

- Set each bar's final height in CSS (percent of the largest value), then animate `scaleY` from 0 with `transformOrigin: "50% 100%"`. Never tween `height`.
- 0.15-0.25 s between bars, `expo.out` 0.6-0.8 s. Value labels appear after their bar lands.
- The bar that carries the message (this month, best seller) gets the accent; the rest use the main color muted with `color-mix()`.
- At most 4-6 bars in 9:16. No gridlines, ticks or legend; label the bars directly.

## 6. Line chart

Compute points from the data, build `d`, then draw left to right.

```js
const DATA = [4.1, 6.3, 8.6, 12]; // the user's values, as given
const [W, H, max] = [760, 300, Math.max(...DATA)];
const pts = DATA.map((v, i) => [(i * W) / (DATA.length - 1), H - (v / max) * H]);
const d = pts.map(([x, y], i) => (i ? "L" : "M") + x.toFixed(1) + " " + y.toFixed(1)).join(" ");
const line = document.getElementById("line");
line.setAttribute("d", d);
line.setAttribute("pathLength", "1");
const draw = { attr: { "stroke-dashoffset": 0 }, duration: 1.6, ease: "power2.out" };
tl.fromTo(line, { attr: { "stroke-dasharray": 1, "stroke-dashoffset": 1 } }, draw, 2.0);
```

- The area under the line (a second path closed to the base) fades in as the line nears the end.
- An accent dot appears at the last point exactly as the line arrives, with a small value label beside it. Mark one key moment only (record, turning point); no label on every point.

## 7. Before and after

| Form               | How                                                                              | Example                                  |
| ------------------ | -------------------------------------------------------------------------------- | ---------------------------------------- |
| One metric changes | the same number falls or rises in place, color switching as it passes the target | ROAS 0,8x to 4,6x (`dasbor-iklan` style) |
| Two bars           | "Sebelum" grows, holds briefly, then "Sesudah" grows taller in the accent        | queue time 40 minutes to 10 minutes      |
| Split screen       | two cards left and right (top and bottom in 9:16), wiping from old to new        | manual monthly cost vs. the app          |

During a comparison the title, labels and axis stay still; only values change. Same concept (month to month): keep the same space and style; change style only when the concept changes.

## 8. Readable timing

Context before the number 0.8-1.2 s; count-up 1.2-2 s; hold after landing at least 1.2 s, ideally 1.5-2 s; bars or line drawing 1.5-2.5 s; final chart state (screenshot-worthy) 2-3 s. In a 15 s video: at most 2-3 numbers, or one chart plus one number. One number per scene.

**Before `preview_composition`:** every number matches the user's exactly, including the last count-up frame; sample data says "Contoh ilustrasi" on every frame it appears; bars start at zero and are proportional; labels stay in the safe zone; id-ID format is consistent (thousands dot, decimal comma, Rp attached).

Further reading via `skill_library`: `hyperframes/hyperframes-animation/blueprints/dataviz-countup.md`; `hyperframes/motion-graphics/categories/stat/module.md` and `categories/charts/module.md` (ignore `npx` commands); `hyperframes/hyperframes-animation/rules/counting-dynamic-scale.md`, `stat-bars-and-fills.md`, `svg-path-draw.md`; `hyperframes/hyperframes-creative/references/data-in-motion.md` (avoid web patterns: pie charts, six-panel dashboards, gridlines). Registry (`search_registry`): "count up", "animated bar chart", "chart story", "conic progress ring", "number wheel".

## Adapted from

- [charlie947/motion-graphics-skills](https://github.com/charlie947/motion-graphics-skills) (MIT, © Charlie Hills), commit `229022e`: `skills/animated-chart/SKILL.md`.
- [iart-ai/data-animation-skills](https://github.com/iart-ai/data-animation-skills) (MIT, © iart.ai), commit `8ce2709`: `skills/chart-animation/SKILL.md`, `skills/chart-animation/references/data-pipeline.md`, `skills/animated-infographic/SKILL.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes-animation/blueprints/dataviz-countup.md`, `skills/motion-graphics/categories/stat/module.md`, `skills/motion-graphics/categories/charts/module.md`.

Changes by Mosio: translated, condensed and adapted to Mosio's composition rules (one paused GSAP timeline, no audio, no CDN). Dropped Remotion, D3, Chart.js, CSV and batch rendering, bar chart races, loops, GIF export and CLI verification; added id-ID number formats (Rp, %, rb/jt/M), the exact-value guard on the last frame, the `pathLength` ring, data-honesty rules and small-business examples. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
