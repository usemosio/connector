# App launch and UI demo

For a POS app, online shop, booking app, local SaaS, online course or a new feature. The product screen is redrawn as HTML/SVG mock UI, and an oversized cursor walks the eye: every click causes the next state. Start from the `peluncuran-aplikasi` style (struck-out problem, sliding window, cursor, rising number) or `dasbor-iklan` (metrics, platform cards, line chart).

## Before the storyboard

- **One claim per video**, sayable in 7 words without "dan": "Catat pesanan tanpa buku" passes; "Platform modern untuk efisiensi dan pertumbuhan" fails.
- **List the numbers allowed on screen.** Ask for real figures and their source (users, processing time, app store rating). No list, no numbers: the proof is the mechanism working.
- **Only features that exist.** Redraw the UI to be sharp, but its content must match the product. No "coming soon" shown as current. Do not fake speed: a 3-minute process shows states (loading, done), not 3 seconds.
- Names and data inside the UI are samples ("Bu Sari, 3 item"); label "Contoh tampilan" if they could pass as real customer data.

## Scene flow

| Act      | 15 s              | 25 s  | Content                                                                       |
| -------- | ----------------- | ----- | ----------------------------------------------------------------------------- |
| Problem  | 0-2.3             | 0-3   | A familiar chore, no product ("Masih catat pesanan manual?"), then struck out |
| Product  | 2.3-4.5           | 3-6   | Window or phone slides in, cursor enters from off screen                      |
| Features | 4.5-10            | 6-17  | 2-3 beats (max 4), each one click and one result                              |
| Proof    | (inside features) | 17-20 | A real number rises, or a clear "done" state                                  |
| CTA      | 10.5-15           | 20-25 | Logo collapses into a button, cursor presses, name and contact hold 2 s+      |

Usual roles: hook → fact → step → step → number → cta (the struck-out chore is the hook, the app reveal a fact, each feature a step).

The claim must read within the first 10 s. If the result is instantly visible (receipt prints, chart rises), open on it for 2 s, then go back to the problem.

## Mock UI screens

- Build with `div` and SVG: top bar, list rows, stat cards, buttons, chips. No screenshots; small icons or the user's logo as inline SVG or a small `data:` image.
- One theme object on `:root` (brand, panel, accent, muted text). One accent, used only for: emphasis word, proof number, active step, status change, CTA button.
- Simple frame (three dots, screen title) or a rounded phone. No fake URL bar, no OS logos.
- 9:16: window 900 px wide (90 px sides), 700-900 px tall, UI text at least 40 px. 16:9: window on one side, feature title on the other.
- Morph one shape instead of cutting: a card grows into a detail panel, a button becomes a toast, a row becomes a receipt. When content swaps, old text leaves first and fast (0.12-0.2 s), then new text enters.

## The oversized cursor

- Height 90-130 px on a 1080-wide canvas (about 7% of width full screen, 5% inside a small window). One white arrow with a 2-3 px dark stroke and small `drop-shadow`, your own shape, not a copied OS cursor.
- **Tip, not centre.** Store the tip offset (e.g. 6, 4 px); position = click point minus offset; `transform-origin` at the tip, so the press centres on the touched spot.
- **Enter from off screen** with one decelerating glide (0.4-0.9 s, `power3.out`), never fade in place.
- **Curved path**: tween `x` and `y` with different eases or durations, or `MotionPathPlugin` with one bend. Optional 0.05 s pull-back before leaving. A straight one-ease line feels robotic.
- **Step aside** on beats that are not its own (typing, counting): slide 0.5-0.9 s, `power2.out`, no idle wiggle. **Exit physically** through the nearest edge (`power2.in`, 0.5-0.7 s) before the held ending.
- Move with `x`/`y`, never `left`/`top`; keep the cursor inside the camera layer if the camera moves.

## A click causes something

Three layers at the same moment: cursor presses, target reacts, feedback blooms.

```js
function click(tl, t, target, n) {
  tl.fromTo(
    `#bloom${n}`,
    { opacity: 0, scale: 0.3 },
    { opacity: 1, scale: 0.62, duration: 0.09, ease: "power2.out", immediateRender: false },
    t - 0.1,
  )
    .to("#cursor", { scale: 0.84, duration: 0.1, ease: "power2.in" }, t)
    .to("#cursor", { scale: 1, duration: 0.22, ease: "power2.out" }, t + 0.11)
    .to(target, { scale: 0.94, duration: 0.1, ease: "power2.in" }, t)
    .to(target, { scale: 1, duration: 0.3, ease: "back.out(1.8)" }, t + 0.11)
    .to(`#bloom${n}`, { opacity: 0, scale: 1.15, duration: 0.34, ease: "power2.out" }, t)
    .fromTo(
      `#ring${n}`,
      { opacity: 0.75, scale: 0.34 },
      { opacity: 0, scale: 1.5, duration: 0.42, ease: "power2.out", immediateRender: false },
      t,
    );
}
```

- Press and release 1:2 (0.1 then 0.22 s). `immediateRender: false` keeps bloom and ring off frame 0. Never start two tweens on the same property at the exact same time; leave 0.01 s.
- Bloom about 1x the button width, ring about 0.7x; bigger reads as a halo.
- **Every click changes something in the same second**: menu opens, toggle flips, row ticks, modal rises, number starts counting. No big change starts by itself. Focusing an input moves only the cursor.

## Camera and zoom

- Wrap window and cursor in `#camera`. Zoom with `scale` and `transformOrigin` at the centre of the control about to be clicked: that point stays put, so cursor and ring coordinates stay right. Scale 1.1-1.6 in 9:16, never above 2.
- Order: zoom finishes **before** the cursor arrives, hold during the click, release after the result shows. Move camera or cursor, rarely both.
- Check bounds first: `newBottom = bottom + (bottom - originY) × (scale - 1)` must stay above the guide's bottom safe zone; check the other edges too.
- Callout: one short noun label ("Struk via WA") plus a drawn line or circle (`scaleX` or `DrawSVGPlugin`), in with the zoom, out before moving on. One at a time.
- Spotlight: an element over the target with `box-shadow: 0 0 0 2000px rgba(0,0,0,.45)` (no blur). For rack focus use opacity 0.5, not `filter: blur` on large areas.
- Light 3D tilt is fine: `transformPerspective` 2200-2800, `rotationY` -7 to -1.5 deg across the scene, `power1.inOut`, on an inner node; zoom on the outer node.

## Feature beats (3-4 s each)

| Step             | Time                     | Job                  |
| ---------------- | ------------------------ | -------------------- |
| Hold on screen   | 0.5-1 s                  | viewer recognises it |
| Cursor to target | 0.4-0.8 s                | curved, decelerating |
| Click and change | 0.2-0.3 s                | see above            |
| Zoom and callout | in 0.6 s, hold 1.5-2.5 s | one benefit, a noun  |

Shorten by cutting features, not reading time. Cheap, clear changes: rows stagger in (0.2-0.3 s), tick with `back.out(2)`, toggle knob slides as the track recolours, tab indicator whose leading edge outruns the trailing edge (`x` and `scaleX` with different durations), modal up from below, toast down from above, receipt rolls out.

## Proof and numbers

- Count up per `../../video-composition/craft/numbers.md`: `power3.out` 1.2-1.8 s, land exactly on the real figure, `tabular-nums` in a fixed-width box.
- Round down only ("12.000+" from 12,480); rounding up is inventing. Client logos only with permission, one at a time in one swapping shape, not a "trusted by" row.

## Ending: logo collapses into the button

1. Logo or app name holds centre 1-1.5 s.
2. Logo shrinks and fades exactly as the CTA button grows from the same centre (same `transform-origin`, button `position: absolute`): one object changing.
3. Cursor glides in, lands slightly off the button centre (feels human-aimed).
4. Cursor and button press together (one tween, two targets), release with a ring.
5. Cursor exits; button, name and real URL or contact hold at least 2 s, then a 0.5 s fade.

## More

`skill_library`: `hyperframes/hyperframes-animation/blueprints/cursor-ui-demo.md`, `.../blueprints/cta-morph-press.md`, `.../examples/cta-morph-press.html`, `.../rules/cursor-click-ripple.md`, `.../rules/coordinate-target-zoom.md`, `.../rules/press-release-spring.md`, `.../rules/cursor-drag.md`. `search_registry`: "oversized cursor", "ui focus zoom", "tabs indicator", "cursor drag".

## Check before sending

- 7-word claim read in the first 10 s; one idea per scene; every number from the user; every feature real.
- Cursor tip on target at each click, and each click changes something in that second.
- Zoom never pushes text into the safe zones; no large full-screen blur. Cursor leaves before the ending; ending holds 2 s+; timeline length equals `data-duration`.

## Adapted from

- [AbubakrChan/product-launch-motion](https://github.com/AbubakrChan/product-launch-motion) (MIT, © Abubakr Chan), commit `b25b163`: `SKILL.md`, `references/technique.md`, `references/prompts.md`, `legacy/references/05-camera-3d-cursor.md`, `legacy/references/02-story-and-truth.md`. Only the MIT-licensed documentation text was adapted; sample films, stills, brand logos and other third-party material (see that repo's NOTICE) are not used.
- [iart-ai/ecommerce-video-skills](https://github.com/iart-ai/ecommerce-video-skills) (MIT, © iart.ai), commit `2bd0dd1`: `skills/product-demo-video/SKILL.md`, `skills/product-demo-video/references/annotation-and-transitions.md`, `skills/product-demo-video/references/screenshot-demo.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `.claude/skills/oversized-cursor/SKILL.md`, `skills/hyperframes-animation/blueprints/cursor-ui-demo.md`, `skills/hyperframes-animation/blueprints/cta-morph-press.md`.

Changes by Mosio: condensed and merged into one flow that fits Mosio's composition rules (paused GSAP timeline, transforms instead of `left`/`top`, no audio, no CDN). Music and SFX, beat grids, the Playwright/ffmpeg pipeline, closed-form `seek(t)` springs, screenshots and browser frames with URLs, and content-swap blur were dropped or replaced with GSAP eases and opacity; added act tables for 15 and 25 s, honesty rules for small businesses (number list, real features, sample labels) and Indonesian examples. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
