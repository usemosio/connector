# Kinetic type

Use when the text is the star: slogans, openers, announcements, CTAs. Recipes for technique and rhythm; the hard rules (paused timeline, word limit, hold times) stay in `get_composition_guide`. Full example: the `kinetic-type` style.

## 1. Set the type before moving it

- Headlines: `line-height` 0.9-1.1, slightly tight `letter-spacing` (-0.01em to -0.03em), one weight.
- Indonesian has long words ("pengiriman", "berlangganan"). Make sure the longest word fits the safe-zone width at the chosen size; condensed fonts like Bebas Neue help. Never hyphenate.
- One emphasis word per phrase, marked by accent color or size, not both on every line.

| Split unit | Feel          | Use for                                      |
| ---------- | ------------- | -------------------------------------------- |
| Line       | calm, premium | 2-3 line taglines; cafes, boutiques, clinics |
| Word       | energetic     | slogans, CTAs, Reels/TikTok openers          |
| Letter     | busiest       | one or two short words only: "PROMO", "BARU" |

## 2. Split the text yourself (default)

Wrap pieces in `span`s inside an `overflow: hidden` mask. For lines, write the lines in the HTML (`.line > span`): automatic line splitting depends on loaded fonts.

```css
.m {
  display: inline-block;
  overflow: hidden;
  vertical-align: top;
  padding-bottom: 0.12em;
  margin-bottom: -0.12em;
}
.s {
  display: inline-block;
}
```

`padding-bottom` keeps descenders (g, j, y) from being clipped by the mask.

```js
// Split an element into masked words or letters; returns the inner spans.
function splitWords(el, mode = "word") {
  const text = el.textContent.trim();
  const parts = mode === "letter" ? [...text] : text.split(" ");
  const span = (className, textContent = "") =>
    Object.assign(document.createElement("span"), { className, textContent });
  el.textContent = "";
  return parts.map((p, i) => {
    const s = span("s", p === " " ? "\u00a0" : p); // non-breaking space keeps letter gaps
    el.appendChild(span("m")).appendChild(s);
    if (mode === "word" && i < parts.length - 1) el.appendChild(document.createTextNode(" "));
    return s;
  });
}
```

**Available GSAP plugins** (load `vendor/<Name>.min.js`, then `gsap.registerPlugin`):

- `SplitText`: `chars` and `words` are safe. Avoid `lines`: line breaks depend on loaded fonts, while Mosio's timeline must be built and registered as soon as the script runs. Write lines yourself as one `<span>` per line.
- `TextPlugin`: swaps text over time (`{ text: "..." }`), fine for a short even typewriter.
- `ScrambleTextPlugin`: do not use. Its random letters are unseeded, so the render can differ from the preview. Build your own scramble with seeded mulberry32 picking letters from timeline time.
- `CustomEase`, `EasePack`: custom curves when `expo.out` is not right.

## 3. Four go-to reveals

| Reveal           | On the timeline                                                                        | Note                                          |
| ---------------- | -------------------------------------------------------------------------------------- | --------------------------------------------- |
| Mask rise        | `fromTo(span, { yPercent: 110 }, { yPercent: 0 })`                                     | cleanest and cheapest; default                |
| Directional wipe | `fromTo(el, { clipPath: "inset(0% 100% 0% 0%)" }, { clipPath: "inset(0% 0% 0% 0%)" })` | letters stay still; good for prices and names |
| Letter stagger   | `fromTo(letters, { yPercent: 60, opacity: 0 }, { ..., stagger: 0.03 })`                | short words only                              |
| Blur in          | `filter: blur(8px)` to `blur(0px)` plus opacity                                        | CPU-heavy: small elements, 8 px max, brief    |

0.4-0.6 s per piece, enter `expo.out` or `power3.out`, exit `power2.in` and faster than the entry. Keep one phrase's whole reveal near 0.8 s; if longer, go up a unit (letters to words, words to lines).

## 4. Word rhythm

Two different things; do not mix them:

- **Stagger inside one reveal** (a phrase opening at once): the guide's stagger range; finer, per unit: lines 0.06-0.1 s, words 0.04-0.07 s, letters 0.02-0.04 s.
- **Word rhythm** (words landing one by one, as if spoken): whole words 2-12 frames apart. Fast 2-4 frames (0.07-0.13 s) for the 0-1.5 s hook and flash sales; headline 4-6 frames (0.13-0.2 s) for headlines and product names; calm 8-12 frames (0.27-0.4 s) for slow-read lines and CTAs.

Write the gaps by hand, uneven: identical rhythm on every word feels mechanical. Leave a slightly longer gap before the emphasis word.

```js
const tl = gsap.timeline({ paused: true });
const words = splitWords(document.getElementById("hook")); // "Laper tengah malam? Martabak dong."
const gaps = [0, 4, 8, 16, 21]; // frames from the first word, deliberately uneven
const up = { yPercent: 0, duration: 0.45, ease: "expo.out" };
words.forEach((s, i) => tl.fromTo(s, { yPercent: 110 }, { ...up }, 0.2 + gaps[i] / 30));
tl.fromTo(words[3], { color: "#fff4e6" }, { color: "#ff6b2c", duration: 0.2 }, 0.2 + 16 / 30); // emphasis word
```

## 5. Kinetic-type beats (one phrase per beat)

From the `kinetic-type-beats` blueprint: big centered text, each beat replacing the last in the same spot, nothing left behind. Arrive, hold, clear.

- One phrase of 1-4 words per beat, one emphasis word (accent color, drawn underline or highlight box).
- Scale contrast: one giant word, small companions. Spend the biggest scale once, usually at the climax.
- The last beat lands and holds until the next scene, with no exit.
- Form A, one slot swapping: the line stays, one word swaps by hard cut (`tl.set`) or delete and retype: "Cocok buat **ngopi**", then **nugas**, then **nongkrong**.
- Form B, stepped statements: each beat fills the screen with its own entrance (bouncing scale, mask rise, wipe); the background may change color with inverted text: "Capek antre?", "Pesan dari rumah.", "Antar 30 menit."
- Tempo: a fast benefit run at ~0.4 s per beat only for 1-2 word phrases; statements that must be understood hold 1.2-1.5 s. Hard cuts (`tl.set` on `opacity` or `visibility`) are the beat.

## 6. Typewriter

Letter by letter only for text that is really typed: a WhatsApp message being written, a Shopee/Tokopedia search box, an order note. Main headlines use whole words. Hand-write an uneven rhythm (some fast letters, a pause after spaces, the last word a bit slower). Store states as `[seconds, text]` and show the last one passed, so seeking or reversing is safe. The cursor does not blink while typing, only while waiting, computed from timeline time.

```js
const TYPED = [
  [0, ""],
  [0.1, "Ka"],
  [0.16, "Kak"],
  [0.45, "Kak, m"],
  [0.52, "Kak, mas"],
  [0.6, "Kak, masih"],
  [0.95, "Kak, masih ada"],
  [1.4, "Kak, masih ada stok?"],
];
const [el, cursor] = ["typed", "cursor"].map((id) => document.getElementById(id));
const p = { t: 0 };
const showTyped = () => {
  let s = "";
  for (const [t, x] of TYPED) if (p.t >= t) s = x;
  el.textContent = s;
  cursor.style.opacity = p.t < 1.45 || Math.floor(p.t * 2.5) % 2 === 0 ? 1 : 0; // blinks only while waiting
};
tl.to(p, { t: 2.4, duration: 2.4, ease: "none", onUpdate: showTyped }, 3.0);
```

Retyping (delete, then a new word) is just more states in the same list. `TextPlugin` is fine for a short even line.

## 7. Reading time

Beyond the guide's minimum hold, allow about 0.3 s per word plus 0.5 s (5 words is about 2 s). While text holds, the background lives but the text does not wobble. Never two headlines in one spot at once: the old one leaves first, or with about 2 frames of overlap as the new one enters.

**Before `preview_composition`:** masks fully cover at the start (no letter tips peeking) and never clip descenders; every word fits the safe zone in every size made; every span gets its start state from `fromTo`, not CSS; word gaps are uneven, one emphasis word per phrase.

Further reading via `skill_library`: `kinetic-typography/kinetic-typography/SKILL.md` and `references/reveal-recipes.md` (translate their CSS @keyframes, Framer Motion and Remotion examples into timeline tweens); `hyperframes/hyperframes-animation/blueprints/kinetic-type-beats.md` and `typewriter-reveal.md`; `hyperframes/hyperframes-animation/rules/discrete-text-sequence.md` and `kinetic-beat-slam.md`. Registry (`search_registry`): "kinetic type swap", "kinetic center build", "per word rise", "typewriter", "caret swap", "headline slam".

## Adapted from

- [iart-ai/kinetic-typography-skills](https://github.com/iart-ai/kinetic-typography-skills) (MIT, © iart.ai), commit `fccc94b`: `skills/kinetic-typography/SKILL.md`, `skills/kinetic-typography/references/reveal-recipes.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes-animation/blueprints/kinetic-type-beats.md`, `skills/hyperframes-animation/blueprints/typewriter-reveal.md`.
- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana), commit `8c8ec0f`: `SKILL.md` (only the word-rhythm law: whole words 2-12 frames apart, letter typing only in input fields with an uneven rhythm).

Changes by Mosio: translated, condensed and adapted to Mosio's composition rules (one paused GSAP timeline, no audio, no CDN). Dropped CSS @keyframes, Framer Motion, Remotion, After Effects, variable fonts, web accessibility, the `?t=` harness and Playwright; added a text-splitting helper, notes on the GSAP plugins Mosio ships, rhythm in frames, a state-list typewriter and small-business copy examples. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
