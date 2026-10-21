# Motion craft

Read after the storyboard is approved and before writing tweens. The technical rules stay in `get_composition_guide`; this file is what good motion looks like inside them. Cut recipes and their code: `transitions.md`.

## 1. Move vocabulary

Name the move each beat needs, then use its recipe; one `power1.inOut` for everything makes it all float.

| Move               | What it is                                              | On the timeline                                                                                                                                                                                                                                        |
| ------------------ | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Snap               | jumps size or position in 1-2 frames, then rests        | `back.out(2)` 0.15-0.25 s, or `tl.set` to 90% then settle the last 3-10% over 7-14 frames                                                                                                                                                              |
| Squash and stretch | flattens on impact, stretches when fast                 | on contact `scaleX: 1.2, scaleY: 0.8` for 2-3 frames, then back to 1; keep the area (`scaleY ≈ 1/scaleX`, within 10%); `transformOrigin` at the contact point (glass base, box bottom)                                                                 |
| Smear              | stretches along its path on the fastest frames, no blur | 1-3 frames of `scaleX` 1.3-1.6 along the motion; for diagonals, use a wrapper rotated to the motion angle. Cheap on CPU, unlike blur                                                                                                                   |
| Overshoot          | passes the target a little, comes back                  | `back.out(1.4-1.7)`; small and late (4-8%) beats a hard bounce; at most one per scene                                                                                                                                                                  |
| Elastic settle     | wobbles a few times, then rests                         | `elastic.out(1, 0.35)` only for illustrated objects landing (mascot, sticker, product icon); once per video, never on text or prices                                                                                                                   |
| Anticipation       | small wind-up the opposite way                          | 0.1-0.35 s at 10-20% of the main move (`power2.in`), then the main move `power3.out`; key moves only                                                                                                                                                   |
| Iris / mask wipe   | new scene opens through a circle or curtain             | `clipPath: "circle(0px at 50% 60%)"` to a radius past the farthest corner (`Math.hypot` to all 4 corners, times 1.05); open `power2.out`, close `power2.in`, 0.3-0.5 s. Curtain: `inset(100% 0% 0% 0%)` to `inset(0%)`. The old scene stays underneath |
| Morph chain        | one shape becomes the next while moving                 | same-structure SVG paths: `attr: { d }`; different structure: `MorphSVGPlugin`; circle to rounded box: tween `borderRadius` or `rx`. Chat bubble into button, dot into logo                                                                            |
| Follow-through     | parts lag, then stop later                              | children (label, shadow, price tag) 2-4 frames (0.05-0.15 s) behind the parent, stopping 0.1-0.2 s later, with a different ease                                                                                                                        |
| Hard stop          | firm stop on the punchline                              | `power4.out` into rest; nothing but the background moves for 0.3-0.6 s                                                                                                                                                                                 |
| Enter big          | stronger than growing from zero                         | opening logo or word starts at 1.4-3x and shrinks to size (snap or `expo.out`, 0.5-0.8 s), once per video. Grow from small only when the growth is the content (a card that becomes the next scene), from 0.6-0.9, never 0                             |

## 2. Timing and easing

**Land slow, leave fast.** Enter with `expo.out` or `power3.out` (most of the distance in the first 4-8 frames, then a long tail). Exit with `power2.in` or `power3.in`, at peak speed exactly on the cut, in about 65-75% of the entry time. `inOut` is for objects moving within the frame, not for entering or leaving. Longer travel needs a bit more time (2x the distance is about 1.3x the time); crossing more than a third of the frame with no change in direction or speed feels like a slide.

| Move (30 fps, 1 frame = 0.033 s) | Frames      | Seconds        |
| -------------------------------- | ----------- | -------------- |
| Snap, hard cut, button press     | 2-3         | 0.07-0.1       |
| Opacity helping an entrance      | 4-6         | 0.13-0.2       |
| Standard (word, card, icon)      | 10-12       | 0.33-0.4       |
| Headline word, main card         | 12-18       | 0.4-0.6        |
| Logo, hero product, big number   | 18-24       | 0.6-0.8        |
| Background drift                 | whole scene | `ease: "none"` |

**Stagger.** Letters 1-2 frames (0.03-0.05 s), words 2-4 frames (0.06-0.12 s), the closing stack (name, CTA, contact) about 8 frames (0.25 s). One wave lasts 0.5 s at most. Keep gaps slightly uneven, with the biggest gap before the key word. Each member lasts about 2x the gap so the tails overlap.

**No dead frames, no filler.** Something always lives (the guide's slow background, a running line, a ticking number). But a breathing background is not content: a scene that finished entering with 2 s left needs more story (a second word, the price, a detail), not wobble. Designed stillness is fine: 0.3-0.75 s before a big moment (price appears, logo lands) makes it land. Hold times follow the guide; a phrase of about 20 letters needs 1.2 s in total.

**One motion system per video.** Pick a personality and keep it:

| Personality | Fits                              | Move duration | Main ease                | Overshoot                 |
| ----------- | --------------------------------- | ------------- | ------------------------ | ------------------------- |
| Playful     | street snacks, toys, trendy cafes | 0.25-0.4 s    | `back.out(1.6)`          | 8-15%, one visible spring |
| Premium     | clinic, property, skincare        | 0.5-0.8 s     | `expo.out`, `power2.out` | 0%                        |
| Clean       | local SaaS, B2B services, courses | 0.3-0.5 s     | `power3.out`             | 0-3%                      |
| Energetic   | flash sale, 12.12 promo           | 0.15-0.35 s   | `expo.out`, hard cuts    | 10-20%                    |

Then lock it: one ease for about 80% of moves, three durations (fast, standard, slow), one entrance pattern, one accent color with one meaning, at most one visible spring. Do not mix blur styles: either sharp (smears and hard cuts, no blur) or small blur on cuts only (`transitions.md`).

## 3. Vector law across cuts

How scene A exits sets how scene B enters:

1. **Same axis.** x stays x, y stays y, scale stays scale.
2. **Same direction, not mirrored.** For scale the direction is the sign: growing is forward, shrinking is back. Exit shrinking, then enter growing from small, is the most common mistake.
3. **Matched speed.** Mirrored eases, same distance and duration: exit `power4.in`, enter `power4.out` (two halves of one `power4.inOut`).
4. **Cut mid-motion.** A is still moving when cut, B is already moving on its first frame. Stopping before the cut, or starting from rest after it, is a dead beat.

**Flow.** One dominant direction per video (default: leftward, like turning to the next page) for every ordinary cut. Other directions carry meaning: up is a conclusion or offer rising above what came before; zoom in (growing) goes deeper into the same idea; zoom out (enter big, shrink) is an arrival: price, product, ending. Two consecutive cuts in opposite directions read as a mistake; a change of direction needs a visible cause (beat, impact, click) or an act break.

**Carrier.** The eye follows objects. The strongest cuts hand one object across the cut at the same position and speed: the product keeps sliding, the chat bubble becomes the button, the price flies into the card. Effects start on the frame of their cause, and big objects bounce slower than small ones.

Write a direction log in the chat storyboard, one line per cut: `2.8 s opener to product: exit left, enter left, cut-the-curve, carrier: glass`. If a line does not fit, fix the plan, not the easing.

## 4. Code architecture

Everything visible at time t depends only on t.

- **Absolute positions** from the storyboard (`tl.to(..., 2.8)`), not `"+=0.2"` chains, so moving one beat does not shift every later beat. Scene times as constants at the top.
- **Initial states from the timeline.** `from`/`fromTo` render their start state when built. `tl.set(el, {...}, 0)` does not render at frame 0 unless given `immediateRender: true`; use that to hide later scenes. Never pass one vars object to two `fromTo` calls (GSAP stores the start state in it); spread a copy, `{ ...to }`.
- **Seeded random, computed once.** Scatter, confetti and star positions come from mulberry32 before the timeline, then get ordinary tweens. No per-step physics: bounces and springs use closed-form eases (`elastic.out`, `CustomBounce`).
- **One group per object**, drawn around its own origin (center or contact point); move the group. Layers in order: `#bg`, `#world`, `#objects`, `#text`. Each scene sits in one wrapper that can move like a camera; never zoom text separately from its camera.
- **Palette as constants**: three colors in `:root`, derivatives via `color-mix()`, no new per-element colors. A comment per scene with its time range.
- **Procedural parts** (many dots, numbers) can run off one clock: `tl.fromTo(clock, { t: 0 }, { t: D, duration: D, ease: "none", onUpdate: () => draw(clock.t) }, 0)`, where `draw(t)` recomputes from scratch.
- **Timeline length** equals `data-duration`: end with `tl.set({}, {}, DURATION)` if the last tween ends early.

```js
const DURATION = 15; // scenes start at 0, 2.8 and 6.2 s
function mulberry32(a) {
  return () => {
    a |= 0;
    a = (a + 0x6d2b79f5) | 0;
    let t = Math.imul(a ^ (a >>> 15), 1 | a);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}
const rnd = mulberry32(12);
const tl = gsap.timeline({ paused: true });
tl.set(["#scene2", "#scene3"], { autoAlpha: 0, immediateRender: true }, 0);

// Scene 1 (0-2.8 s): glass drops (gravity, so ease-in), squashes on landing, settles
tl.fromTo("#glass", { y: -1100 }, { y: 0, duration: 0.42, ease: "power3.in" }, 0.15)
  .to("#glass", { scaleX: 1.2, scaleY: 0.83, duration: 0.07, ease: "power2.out" }, 0.57)
  .to("#glass", { scaleX: 1, scaleY: 1, duration: 0.6, ease: "elastic.out(1, 0.35)" }, 0.64)
  .fromTo("#shadow", { scaleX: 0.4 }, { scaleX: 1, duration: 0.3, ease: "power3.out" }, 0.6); // follow-through

// Sugar burst: seeded positions, computed once
document.querySelectorAll(".grain").forEach((el, i) => {
  const to = { x: (rnd() - 0.5) * 520, y: -180 - rnd() * 260, opacity: 0, duration: 0.7 };
  tl.fromTo(el, { x: 0, y: 0, opacity: 1 }, { ...to, ease: "power2.out" }, 0.6 + i * 0.012);
});

// ... scene 2 (2.8-6.2 s) and scene 3 (6.2-15 s), cuts as in the direction log
tl.set({}, {}, DURATION);
window.__timelines = window.__timelines || {};
window.__timelines["main"] = tl;
```

**Before `preview_composition`:** every move has a role (enter, exit, drift, number) and a fitting ease, at most one spring shows, every cut matches the direction log, and the timeline length equals `data-duration`.

Ready-made recipes via `skill_library`: `hyperframes/hyperframes-animation/rules-index.md` (e.g. `rules/spring-pop-entrance.md`, `rules/svg-path-draw.md`) and blueprints such as `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md`. Mosio styles that already use these patterns: `kinetic-type`, `product-hero`, `flash-sale` (`get_composition_guide` with `style`).

## Adapted from

- [lovisdotio/motion2code](https://github.com/lovisdotio/motion2code) (MIT, © Lovis Odin), commit `0f00328`: `skills/motion2code/SKILL.md`, `skills/motion2code/references/motion-vocabulary.md`, `skills/motion2code/references/animation-architecture.md`.
- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana), commit `8c8ec0f`: `SKILL.md`, `references/motion.md`.
- [LottieFiles/motion-design-skill](https://github.com/LottieFiles/motion-design-skill) (MIT, © LottieFiles), commit `f9a8a04`: `skills/motion-design/SKILL.md`, `skills/motion-design/reference/timing-easing-tables.md`, `skills/motion-design/director/choreography.md`, `skills/motion-design/director/disney-principles.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `.claude/skills/motion-doctrine/SKILL.md`.

Changes by Mosio: translated, condensed and merged into one guide for Mosio's paused GSAP timeline (no audio, no CDN). Dropped video-study and render scripts (Python, ffmpeg, Playwright), motionmaxxing's runtime and measured eases, shutter blur and blur smears, Three.js, sound sync, ledger files and Seam Gate scripts; softened the "idle wobble" ban to fit Mosio's living-background rule and limited elastic to illustrated objects. Added a personality table for small-business types, the `immediateRender` note for initial states, and Mosio code examples. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
