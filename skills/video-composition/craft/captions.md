# Captions: the word-by-word engine

Use when the captions carry the story: the `visual-story` type (`../../video/types/visual-story.md`) and any silent piece told word by word. Mosio videos have no audio, so word times come from a reading-pace plan, not from a voice. Everything runs on the one paused timeline; the hard rules stay in `get_composition_guide`. Full example: the `visual-story` style.

## 1. Tokens and phrases

- A **token** is one word as shown, with its punctuation ("celah:", "sama,"). A multi-word term boxed together is one token joined by a no-break space (`"segi\u00a0enam?"`), so it never wraps and is boxed as one.
- A **phrase** is 1-5 tokens, broken at commas and sentence ends, 2 lines at most at 920 px (about 28 characters a line at 60 px; check the longest one in the preview). At most one boxed token per phrase, about one per 1-2 sentences: the number, the key term or the payoff word. The guide's 7-word cap counts the phrase plus any tag or counter label on screen; shorten the phrase while a label is up.

## 2. Reading-pace plan

Write the script as data and derive every time from it: 2.3-2.8 words per second, a phrase gap of 0.25-0.4 s, a short hold after the boxed token. Motion uses the same times through `at()`, so it lands on its word. Compute the total once (`CAPTIONS_END`, the payoff hold, then the ending of 2 s or more) and write it into `data-duration`.

```js
// One array per phrase. "*" marks the boxed token; \u00a0 joins a multi-word token.
const SCRIPT = [
  ["Kenapa", "sarang", "lebah", "bentuknya", "*segi\u00a0enam?"],
  ["Cuma", "ada", "*tiga", "bentuk"],
];
const PACE = { start: 0.2, wps: 2.5, gap: 0.3, sentenceGap: 0.4, emHold: 0.25, minPhrase: 1.2 };
function planCaptions(script, o = PACE) {
  const [W, PHR] = [[], []];
  let t = o.start;
  for (const phrase of script) {
    const p = { a: W.length, b: 0, em: -1, t0: t };
    for (const raw of phrase) {
      const text = raw.replace(/^\*/, "");
      if (text !== raw) p.em = W.length;
      W.push({ text, t: Math.round(t * 30) / 30 }); // whole frames
      t += text.split("\u00a0").length / o.wps + (text.length > 9 ? 0.1 : 0);
      if (text !== raw) t += o.emHold;
    }
    PHR.push(Object.assign(p, { b: W.length - 1 }));
    t = Math.max(t, p.t0 + o.minPhrase) + (/[.?!]$/.test(W[p.b].text) ? o.sentenceGap : o.gap);
  }
  return { W, PHR, end: t };
}
const { W, PHR, end: CAPTIONS_END } = planCaptions(SCRIPT);
const at = (text, n = 0) => W.filter((x) => x.text === text)[n].t; // a typo throws: fix it
```

## 3. Token states (30 fps, 1 frame = 0.033 s)

| State      | What happens                                                                                       |
| ---------- | -------------------------------------------------------------------------------------------------- |
| Enter      | on its time: opacity 0.6 to 1 and `blur(8px)` to 0 over 3 frames (0.1 s), linear; no move or scale |
| Active     | full contrast (#fff on dark), weight 700                                                           |
| Spent      | one frame before the next token enters: 45% white, weight 400                                      |
| Emphasised | stays bold and white, boxed, until the phrase ends                                                 |
| Phrase end | hard cut 3 frames before the next phrase's first token; no fade                                    |

**Re-centring**: bold words are wider, so never reflow one live line. Build one pre-laid-out line per reveal state (tokens a..k) in the same spot and swap them with `tl.set` on opacity. Each state is centred exactly; nothing animates `display`.

## 4. The emphasis box (Figma-style selection)

Two round handles pop at top-left and bottom-right (`back.out(2.4)`, 3 frames, 2 frames after the token enters). The stroke draws from the handles: horizontal edges over 3 frames, then vertical edges over 3 frames. The fill fades in behind the word over 4 frames and stays until the phrase ends. Build the box in every line state from the boxed token on, with the same tweens at the same times, so later states show it finished. Type: Inter (Google Fonts, 400 and 700), 60 px, line-height 1.22, letter-spacing -0.012em. Without `isolation: isolate` on the word, the fill at `z-index: -1` drops behind the scrim and the world.

| Class                   | Style                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- |
| `#cap` `.cl`            | band absolute at left 80, top 280, 920 × 220; each line absolute at 0 0, 920 px, centred, opacity 0         |
| `.w` `.w.on`            | inline-block, relative, `isolation: isolate`, 400 at `rgba(255,255,255,.45)`; `.on` is #fff at 700          |
| `.box` `.fill`          | box absolute, `inset: -3px -9px -5px -9px`; fill inset 0, `z-index: -1`, opacity 0, #151e36 (light #d7e1f4) |
| `.et` `.eb` `.er` `.el` | 2 px edges, #2f66b8 (light #3266bd); `transform-origin` 0 50%, 100% 50%, 50% 0, 50% 100%                    |
| `.h1` `.h2`             | 14 px circles #257bff, `box-shadow: 0 0 0 2px #fff`, at -7 px top-left and bottom-right                     |

```js
const F = 1 / 30;
const SPENT = { color: "rgba(255,255,255,0.45)", fontWeight: 400 };
const PARTS = ["fill", "e et", "e er", "e eb", "e el", "h h1", "h h2"];
const lin = (frames) => ({ duration: frames * F, ease: "none" });
const BLUR = { opacity: 0.6, filter: "blur(8px)" };
function addBox(word, t0) {
  const box = word.appendChild(document.createElement("span"));
  box.className = "box";
  box.innerHTML = PARTS.map((c) => `<span class="${c}"></span>`).join("");
  const q = (c) => box.querySelectorAll(c);
  tl.fromTo(q(".h"), { scale: 0 }, { scale: 1, duration: 3 * F, ease: "back.out(2.4)" }, t0);
  tl.fromTo(q(".et, .eb"), { scaleX: 0 }, { scaleX: 1, ...lin(3) }, t0 + F);
  tl.fromTo(q(".er, .el"), { scaleY: 0 }, { scaleY: 1, ...lin(3) }, t0 + 4 * F);
  tl.fromTo(q(".fill"), { opacity: 0 }, { opacity: 1, ...lin(4) }, t0 + F);
}
PHR.forEach((p, pi) => {
  const end = pi + 1 < PHR.length ? W[PHR[pi + 1].a].t - 3 * F : CAPTIONS_END;
  for (let k = p.a; k <= p.b; k++) {
    const line = document.getElementById("cap").appendChild(document.createElement("div"));
    line.className = "cl";
    for (let i = p.a; i <= k; i++) {
      const s = line.appendChild(document.createElement("span"));
      s.className = i === k || i === p.em ? "w on" : "w";
      s.textContent = W[i].text;
      if (i === p.em) addBox(s, W[i].t + 2 * F);
      if (i < k) line.appendChild(document.createTextNode(" "));
    }
    const active = line.lastChild;
    const tOut = k < p.b ? W[k + 1].t : end;
    tl.set(line, { opacity: 1 }, W[k].t);
    tl.set(line, { opacity: 0 }, tOut);
    tl.fromTo(active, { ...BLUR }, { opacity: 1, filter: "blur(0px)", ...lin(3) }, W[k].t);
    if (k < p.b && k !== p.em) tl.set(active, SPENT, tOut - F);
  }
});
```

**Scrim**: a full-frame layer under the band, `linear-gradient(to bottom, rgba(13,10,7,.9) 0%, rgba(13,10,7,.82) 22%, rgba(13,10,7,0) 30%)`. Grey spent words over a bright scene are fixed with the scrim, never by brightening them; it is a gradient, not a caption box. Light world: active #111, spent 40% black, a white scrim. The 8 px blur sits on one small word for 3 frames, cheap on the CPU; never blur the whole band.

**Before `preview_composition`:** every token time comes from the plan; no line is visible when the next phrase starts; the boxed token stays bold after later words arrive; the longest phrase fits 2 lines; spent words read over the brightest scene.

## Adapted from

Original to Mosio: adapted from the team's own visual-story skill (the bees and hexagon explainer). Changes by Mosio: made silent (captions carry the story, timed at reading pace, no voice-over or TTS), 2D only (no three.js or WebGL), 30 fps, and fitted to Mosio's composition rules and tools; the Python, CLI and voice pipeline was dropped.
