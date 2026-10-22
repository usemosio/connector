# Chat story

A story told in chat bubbles: one side asks, the other answers, and the last message becomes the CTA. Every flavour runs on the same engine; starting code is the `chat-testimoni` recipe.

| Flavour                  | Asker (right)       | Answerer (left, types first) | Example                                              |
| ------------------------ | ------------------- | ---------------------------- | ---------------------------------------------------- |
| Shop dialogue            | customer            | the shop                     | "Kak, brownies kukus masih ada?" / "Masih, Kak!"     |
| Order flow               | customer            | the shop                     | ask, pick variant, total, transfer, shipped          |
| FAQ                      | customer            | the shop                     | the 2-4 questions the shop's WhatsApp gets most      |
| Content or education Q&A | learner or follower | the creator or expert        | "Kenapa roti cepat berjamur?" / "Karena lembap, Kak" |

Usual roles: hook → problem → fact → proof → cta (the customer's question is `problem`, the shop's answer `fact`, a real review `proof`).

## Honesty (required)

- A made-up shop chat is a **dramatisation**, not a testimonial. Show a small "Ilustrasi" label (at least 40 px, inside the safe zone, visible throughout); never title it "Kata pelanggan kami".
- Testimonials only from a real chat the user provides, with the customer's permission. Mask names and numbers (initials, "Pelanggan, Bandung"). Add no words. Never invent prices, stock, shipping cost, ratings, review counts or contacts. Ask.
- Education Q&A: the answer must be correct. Use facts the user gives or plain common knowledge, no invented statistics; ask before stating health, legal or money claims. The asker is "Ilustrasi" unless it is a real follower question shared with permission.
- Copy no specific app: no WhatsApp/iMessage logo, doodle wallpaper, exact header colour, blue ticks or status bar icons. Generic green or neutral is fine.

## Script first, as data

One array holds the conversation; every time is derived from it, none placed by hand.

```js
const messages = [
  { from: "asker", text: "Kak, brownies kukus masih ada?", lines: 1 },
  { from: "answer", text: "Masih, Kak! Mau berapa kotak?", lines: 1, typing: 0.9 },
  { from: "asker", text: "Dua ya, kirim ke Depok", lines: 1 },
  { from: "answer", text: "Siap, dikirim hari ini juga", lines: 1, typing: 1.2 },
];
```

- `from`: `asker` on the right (the viewer's side), `answer` on the left, always preceded by the typing indicator.
- `lines`: 1 or 2. Bubble height comes from this, so nothing measures the DOM (fonts may not be loaded when the script runs). Confirm the guess in the preview. Optional: `typing` (indicator seconds), `gap` (pause before), `read` (hold after).

## Generic look and layout

- Background: soft brand colour or a slow gradient. Header: initial circle or the user's logo, business or creator name, optional "online"; no call or video icons.
- Bubbles: `max-width` about 78% of the thread, radius 40-48 px, a tail corner (radius 10-12 px) on the sender side of a turn's last bubble. Asker in the primary colour or a `color-mix` of it, answerer white or light grey.
- Bubble text 46-54 px, captions ("Terbaca", time) at least 40 px. One "Hari ini" stamp above the first message; at most one simple tick or "Terbaca" under the last asker bubble.
- 9:16 (1080 × 1920): header about y 260-420, thread y 440-1480 with `overflow: hidden` and 80 px sides, newest bubble landing in the thread's lower third, clear of the bottom safe zone. 1:1 and 4:5: shorter thread, 3-4 messages. 16:9: thread on one side, title, product or topic visual on the other.

## 7 words, pacing

- One bubble holds at most 7 words; split longer answers into 2-3 bubbles from the same sender (0.2-0.3 s apart, no indicator). Education answers usually need this. When a bubble lands, the one two steps back dims to opacity 0.35-0.45 or has scrolled out: only the latest exchange reads clearly.
- One message, one beat: pause, typing (answerer only), bubble. Never the whole chat at 0 s. 4-8 messages for 15-25 s; more means cut the story.

| Part         | Value                  | Note                                      |
| ------------ | ---------------------- | ----------------------------------------- |
| Pause before | 0.3-0.5 s              |                                           |
| Typing       | 0.5-0.9 s              | 1.5-2.5 s before a surprise or key answer |
| Bubble in    | 0.35-0.45 s            | `back.out(1.6)` from the sender corner    |
| Read         | 1.2 s + 0.2 s per word | the guide's 1.2 s minimum                 |
| Ending       | 2 s+ held              | CTA, name, contact                        |

## Core code

The indicator (`#typing`, three dots `i`) and all bubbles sit in `#body`, inside the `overflow: hidden` thread. Bubbles are `position: absolute` with a computed `top` (set once, never animated). `transform-origin` goes in CSS (`.left { transform-origin: 0 100% }`, `.right { transform-origin: 100% 100% }`); it is not a starting transform, so it is safe.

```js
const tl = gsap.timeline({ paused: true }),
  body = document.getElementById("body"),
  els = [];
const THREAD = 1040,
  GAP = 28,
  LH = 66,
  PAD = 68; // height = PAD + lines × LH
let t = 1.0,
  y = 0;
messages.forEach((m, i) => {
  const el = document.createElement("div");
  el.className = "bub " + (m.from === "answer" ? "left" : "right");
  el.textContent = m.text; // textContent: & and < are safe
  el.style.top = y + "px";
  body.appendChild(el);
  els.push(el);
  const h = PAD + m.lines * LH;
  t += m.gap ?? 0.4;
  // Auto-scroll as a tween: keep the new bubble inside the thread.
  if (y + h > THREAD) tl.to(body, { y: THREAD - (y + h), duration: 0.5, ease: "power3.out" }, t);
  if (m.from === "answer") {
    const k = m.typing ?? 0.8,
      reps = Math.max(1, Math.floor(k / 0.18) - 1) | 1; // odd: dots end down
    tl.set("#typing", { y }, t)
      .fromTo(
        "#typing",
        { opacity: 0, scale: 0.8 },
        { opacity: 1, scale: 1, duration: 0.2, ease: "power2.out" },
        t,
      )
      .fromTo(
        "#typing i",
        { y: 0 },
        { y: -12, duration: 0.18, yoyo: true, repeat: reps, stagger: 0.08, ease: "sine.inOut" },
        t,
      )
      .to("#typing", { opacity: 0, duration: 0.1 }, t + k);
    t += k;
  }
  tl.fromTo(
    el,
    { opacity: 0, y: 24, scale: 0.85 },
    { opacity: 1, y: 0, scale: 1, duration: 0.4, ease: "back.out(1.6)" },
    t,
  );
  if (i >= 2) tl.to(els[i - 2], { opacity: 0.4, duration: 0.3 }, t);
  t += 0.4 + (m.read ?? 1.2 + 0.2 * m.text.split(" ").length);
  y += h + GAP;
});
// t is now the ending start. Compute the total, write it to data-duration, end the last 0.5 s fade exactly there.
```

## The last message is the CTA

The answerer's last bubble carries it: "Pesan sekarang, Kak" with the user's real contact, or for content "Follow untuk tips berikutnya" with the real handle. It grows to centre (scale 1.15-1.3, `power3.inOut`) while the others fade; or the thread wipes away (`clipPath`) and the bubble becomes a primary-colour pill button. Name and contact hold at least 2 s, then fade in the last 0.5 s.

## Variations and components

- **Order flow**: small cards between bubbles (order summary, SVG product illustration, total), in with `back.out` like a bubble. Prices only from the user.
- **FAQ or education Q&A**: each question followed by its answer, longer read pause on answers; 3 questions fill 20 s. A myth-vs-fact twist works as a surprise answer.
- **Real chat testimonial**: the `chat-testimoni` pattern (bubble typed letter by letter, then stars). Stars and scores only with a real rating.
- **Surprise answer**: typing runs 1.5-2.5 s, then the punchline bubble lands slightly bigger.
- Components: `search_registry` "chat" or "typing" returns `chat-message`, `chat-thread`, `typing-indicator`; they imitate iMessage (tail, "Delivered"), so restyle to the generic look first. Letter-by-letter typing: `hyperframes/hyperframes-animation/blueprints/typewriter-reveal.md` via `skill_library`.

## Check before sending

- One `messages` array drives everything; the indicator shows before every answer and vanishes as the bubble lands.
- The newest bubble stays out of the bottom safe zone; scrolling is smooth.
- "Ilustrasi" label on made-up chats; no app's logo or exact look; education facts checked. Ending holds 2 s+ and the timeline length equals `data-duration`.

## Adapted from

- [iart-ai/text-message-video-skills](https://github.com/iart-ai/text-message-video-skills) (MIT, © iart.ai), commit `3a800e1`: `skills/text-message-animation/SKILL.md`, `skills/text-message-animation/references/bubble-recipes.md`, `skills/text-message-animation/references/data-driven.md`.

Changes by Mosio: condensed and rewritten from Remotion to one paused GSAP timeline (auto-scroll as a tween, bubble height from data, no audio, no CDN). Per-message sounds, iMessage/WhatsApp look-alikes, CSV batch rendering and render commands were dropped; added honesty rules ("Ilustrasi" label, testimonials only from real chats, correct answers for education), the 7-word bubble rule, the CTA from the last message, the education Q&A flavour and Indonesian examples. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
