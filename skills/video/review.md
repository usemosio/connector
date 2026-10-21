# Review before and after the preview

Self-critique, twice per round: before `preview_composition` or `preview_template` (on the storyboard and code) and after the preview shows or the user comments. Technical rules stay in the guide and the lint; this judges whether the video is good, honest and does its job.

## How to look

Passing lint is not good. A clean, smooth version still fails if it feels generic, empty or slow. Judge as a stranger watching once, on a phone, on mute, while scrolling.

1. **Judge what shows, not what you meant.** If the preview returns frame images, look at them. If not, walk the timeline at key times: frame 0, each cut (0.1 s before and after), the middle of each scene, the last 2 s. Ask: what is on screen at that second?
2. **Describe each scene in one sentence:** what is on screen, what the hero is, and whether it would pass as a slide or a web page. No one-sentence answer means no hero; "a card on a flat color" is a slide.
3. **Four looks:** thumbnail size (still readable?), cover the logo (fits a competitor too?), cover the text (any picture left?), a mid-cut frame (two half states, an empty frame, a white flash?).
4. **Fix in order:** wrong or broken, then idea and message, then links between scenes, then hierarchy and scale, then timing, then polish. No easing tweaks while the idea is unclear.
5. **Never certify your own work.** Tell the user what you checked and what is still weak; never "sudah sempurna" or "terverifikasi".

## Never ship

A video with any of these is not done. Remove the element; do not defend it.

1. **HUD chrome:** small business name in a corner as a label, wide-tracked capital labels, "01 / 04" counters, timecodes, progress bars, corner brackets, scene labels over the picture.
2. **Slide layout:** a static left-aligned title while content changes, a small kicker above the title, title then body, header and footer, a static "title left, phone right".
3. **Generic UI cards:** rounded cards with a status dot and "label · Rp value", "Selamat pagi, Kak" cards, grey skeleton bars, emoji in place of real icons.
4. **Slogan copy:** three one-word sentences ("Cepat. Murah. Terpercaya."), "Bukan sekadar X, ini Y", "Wujudkan impianmu", "Masa depan belanja", "Solusi terbaik untuk Anda".
5. **Invented stats:** walls of numbers, a big centered number over a small label with no source from the user, "terjual 10rb+" never mentioned.
6. **Same fade-up everywhere:** four or more elements with identical entrances (same opacity and y, duration, ease); the same stagger on every list; a pulse or glow repeated on many elements.
7. **Empty web page:** a small card (about 0.3 screen width) floating on a flat background; flat color with no light, shape or texture; one small object centered for over 1.5 s.
8. **Web-footer ending:** logo, tagline, button and URL stacked like a page footer for no story reason. The Mosio ending is still required (action, name, contact, held at least 2 s), but as the story's last beat: brought in by the previous object or motion, one specific action ("Pesan lewat WA"), no extra tagline.
9. **Crossfades between scenes** (two transparent scenes stacked) and white or black flashes at cuts.

The source caps the end card at 1.4 s. Not in Mosio: the ending holds at least 2 s so the contact can be read and screenshotted.

## What good looks like

1. **The idea belongs to this business:** its real product, how it is served, its materials, the place, the owner's own phrases. Cover the logo: if it still fits the shop next door, the idea came from the category.
2. **The viewer sees something happen.** Claims are shown, not written: ice poured, a WA order arrives then gets packed, before and after, a step done on screen. One specific, real case from the user.
3. **Each beat causes the next.** Objects carry across cuts, motion direction stays consistent (see `../video-composition/craft/motion.md`).
4. **Emphasis is rationed.** One hero per frame, at most three focal points; big needs small around it; at most a quarter of beats are "the point".
5. **Time breathes.** A short pause before what matters, enough reading time, stop before the idea runs out.
6. **One world.** One logic of light, material and color throughout; the background is a surface (gradient, light, shape, texture), not a swatch.
7. **One screenshot-worthy frame,** and it is the most worked-on moment.

Common client rejections: "too plain, just a picture then text", "lots of wasted space", "this part drags", "uneven spacing", "the opening is too dark" (food, health and household products: bright and clear).

## Truth

- Never invent prices, discounts, testimonials, customer names, ratings, units sold, followers, certifications, guarantees, addresses, hours, contacts, or facts in a lesson. All from the user; if missing, ask or leave it out.
- Every illustration carries a readable "Contoh" or "Ilustrasi" label: made-up chats, sample numbers, mock UI, sample reviews.
- Never copy the exact look or logo of another app (WhatsApp, Shopee, Tokopedia). A similar generic style is fine.
- On-screen claims must be true for this business ("dikirim hari ini" only if the user said so). Statements about the video too: never call it AI made. Mosio uses no AI models for images, video or audio; the motion is written as code.

## Purpose on mute

Mosio videos are always silent. A first-time viewer, on mute and at phone size, must be able to answer at the end:

- **Selling (promo, ad, launch):** what is sold and by whom (name visible), one reason to care (taste, price, ease, result), what to do now (chat WA, visit, link in bio, checkout).
- **Teaching (explainer, tips):** the lesson in one sentence, the steps in order, who taught it and what to do next (save, try it, contact).
- **Social content:** what happened or was shown, whose account it is, why to follow or come back.

Test: rewrite the video's message from only the text and images on screen. Vague, or needs sound: the video fails, however good the motion.

## Lazy defaults to question

Not bans. Each is fine when chosen with a one-sentence reason for this business; wrong when used out of habit.

- Gradient text (`background-clip: text`), an accent bar on a card's left edge.
- Cyan on dark, purple-to-blue gradients, neon accents; pure `#000` or `#fff` (tint toward the accent).
- "Premium pastiche": near-black, radial glow, one italic serif word. Dark because the brand is dark, or because dark feels expensive?
- Grids of identical cards, everything centered with equal weight.
- The first association of the name or category: floating coffee beans for a café, lightning for "kilat", a rocket for "launch".
- Every brief point gets an equal scene; nothing is "the point".
- States, not events: everything appears, then sits.
- A new effect in every scene to show off technique.
- A flat drawing followed by separate depth objects that pop in: often the same drawing could stand up in place and hand its key element over instead. Keep the cut if it is a deliberate new world.
- House style as default: giant cropped words, circle wipes at every cut, typed titles with a caret, a dot that becomes the logo.
- A storyboard with motion words ("slams in", "wipe", "zoom") instead of roles and subjects. The user approves content; techniques come from the style plan.
- Content rewritten when only the style changed. Keep the approved beats; redo only the plan.
- A second style built without being asked. Offer it in one line instead.

## Checklist every round

Mark each item **Pass** or **Fail**. A failed Critical item must be fixed before the user presses "Buat video".

| #   | Item                                                                                                                                              | Level       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| 1   | Honest: no invented prices, testimonials, numbers, facts or contacts; illustrations labeled "Contoh"                                              | Critical    |
| 2   | Purpose on mute is clear at the end (see above for the type)                                                                                      | Critical    |
| 3   | Ending: action, name, contact held at least 2 s, inside the safe zone, readable at phone size                                                     | Critical    |
| 4   | Text inside the safe zone, at most 7 words at once, never cropped or colliding, including during transitions                                      | Critical    |
| 5   | Frame 0 is already composed or moving; no empty frame longer than 3 frames, no white flash at cuts                                                | Critical    |
| 6   | None of the never-ship items                                                                                                                      | Important   |
| 7   | One hero per frame; with text covered, every scene still shows a picture                                                                          | Important   |
| 8   | Cuts follow the vector law (axis, direction, speed, cut mid-motion), no crossfades                                                                | Important   |
| 9   | Motion by role: entrances decelerate, exits accelerate; no 4 identical entrances; at most one visible spring                                      | Important   |
| 10  | Timing: text readable at least 1.2 s; no fully frozen frame (background too) over 0.6 s except designed pauses and the ending; scene lengths vary | Important   |
| 11  | Storyboard: beats have roles and subjects, no motion words; content unchanged when only the style changed; no second style built unasked          | Important   |
| 12  | One system: one motion personality, one accent color, at most two font families                                                                   | Opportunity |
| 13  | One screenshot-worthy frame, nameable by its second                                                                                               | Opportunity |

Before the preview: run items 1-11 on the storyboard and code (tween positions, text sizes, colors). After it: run all 13, then compare with the last round, marking each old failure Fixed, Partial or Still there.

Report briefly in the user's language, e.g. "Review putaran 2: 11 dari 13 lulus. Gagal: #5 (kilat putih di 6,2 s karena `#root` tanpa latar), #9 (lima kartu menu masuk dengan fade-up yang sama). Saya perbaiki dua itu dulu." Fix at most the three worst failures per round, biggest impact first. Two or three rounds are usually enough; stop when fixes turn cosmetic.

## Adapted from

- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana), commit `8c8ec0f`: `SKILL.md`, `references/slop.md`.
- [echris6/motion-video-kit](https://github.com/echris6/motion-video-kit) (MIT, © echris6), commit `e4b1c00`: `business-motion-film/SKILL.md`, `business-motion-film/references/quality-bar.md`, `business-motion-film/references/critic-prompts.md`.
- [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) (MIT, © Kyle Zantos), commit `4a9ca87`: `skills/design-motion-principles/workflows/audit.md`, `skills/design-motion-principles/references/anti-checklist.md`, `skills/design-motion-principles/references/audit-checklist.md`.
- [howseen-ai/claude-motion-design](https://github.com/howseen-ai/claude-motion-design) (MIT, © Howseen AI (Raphaël Aubry)), commit `3d90d34`: `skill/motion-design/SKILL.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes-creative/references/house-style.md` (copy in `skill_library`: `hyperframes/hyperframes-creative/references/house-style.md`).

Changes by Mosio: translated, condensed and merged into one review routine for the preview flow (no audio, no render scripts, paused GSAP timeline). Dropped script-computed gates (look.py, lint.mjs, ffmpeg), separate sub-agent critics, NOTE.md and HTML report files, audio and loudness checks, cursor and 3D rules, and the 1.4 s end-card cap (replaced by Mosio's 2 s held ending). Added small-business examples, the "Contoh" label, a purpose-on-mute test per video type, and a scored checklist with Critical, Important and Opportunity levels. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
