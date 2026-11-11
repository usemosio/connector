# Review before and after the preview

Self-critique, twice per round: on the storyboard and code before you build, and on the stills from `inspect_composition` before the user sees `preview_composition` (and again after the user comments). Technical rules stay in the guide and the lint; this judges whether the video is good, honest and does its job.

## How to look

Passing lint is not good. A clean, smooth version still fails if it feels generic, empty or slow. Judge as a stranger watching once, on a phone, on mute, while scrolling.

1. **Judge what shows, not what you meant.** Call `inspect_composition` with `at` at the middle of each beat, plus 0, 0.5, 1 and 2 s for the hook (`hooks.md`), and look at the contact sheet it returns: one still per time plus the last frame, made by the same engine as the render. Read its findings too (text outside its box, covered text, off-canvas, contrast), each with a fix hint; an effect you meant gets `data-layout-allow-overflow` or `data-layout-allow-overlap` instead of a fix. Need a cut or a single moment up close? Inspect again with just those times. Where the tool is not available, walk the timeline in your head at frame 0, each cut, the middle of each scene and the last 2 s. Ask: what is on screen at that second?
2. **Describe each scene in one sentence, as a stranger would,** naming every object: what is on screen, what the hero is, and whether it would pass as a slide or a web page. No one-sentence answer means no hero; "a card on a flat color" is a slide. An object you would have to explain ("those bars are the walls") fails item 2 of the checklist: redraw it until it reads as itself.
3. **Four looks:** thumbnail size (still readable?), cover the logo (fits a competitor too?), cover the text (any picture left?), a mid-cut frame (two half states, an empty frame, a white flash?).
4. **Fix in order:** wrong or broken, then idea and message, then links between scenes, then hierarchy and scale, then timing, then polish. No easing tweaks while the idea is unclear. A video that feels busy: remove about a third of the motion and keep the moves that lead the eye.
5. **At most two inspection rounds**, then show the preview: the user's eyes are the next check. Tell the user in one line what you checked and what is still weak; never "sudah sempurna" or "terverifikasi".

## Never ship

A video with any of these is not done. Remove the element; do not defend it.

1. **HUD chrome:** decoration that dresses the frame as a dashboard: the business or channel name in a corner as a label, wide-tracked capital labels on every scene, timecodes, corner brackets, scene labels over the picture, "01 / 04" counters and progress bars that keep no promise. Allowed because they carry meaning: a progress cue for a count promised on screen ("3 cara ...": a 1/3 chip, `types/explainer.md`), one series chip ("Bagian 2/5"), and brackets on a single "lihat ini" punch-in on footage (`craft/zooms.md`).
2. **Slide layout:** a static left-aligned title while content changes, a small kicker above the title, title then body, header and footer, a static "title left, phone right".
3. **Generic UI cards:** rounded cards with a status dot and "label · Rp value", "Selamat pagi, Kak" cards, grey skeleton bars, emoji in place of real icons.
4. **Slogan copy:** three one-word sentences ("Cepat. Murah. Terpercaya."), "Bukan sekadar X, ini Y", "Wujudkan impianmu", "Masa depan belanja", "Solusi terbaik untuk Anda".
5. **Invented stats:** walls of numbers, a big centered number over a small label with no source from the user, "terjual 10rb+" never mentioned.
6. **Same fade-up everywhere:** four or more elements with identical entrances (same opacity and y, duration, ease); the same stagger on every list; a pulse or glow repeated on many elements.
7. **Empty web page:** a small card (about 0.3 screen width) floating on a flat background; flat color with no light, shape or texture; one small object centered for over 1.5 s.
8. **Web-footer ending:** logo, tagline, button and URL stacked like a page footer for no story reason. The Mosio ending is still required (action, name, contact, held at least 2 s), but as the story's last beat: brought in by the previous object or motion, one specific action ("Pesan lewat WA"), no extra tagline.
9. **Crossfades between graphic scenes** (two transparent text scenes stacked) and white or black flashes at cuts. Mid-crossfade, both scenes show at half strength, so the frame is muddy and neither line can be read; a flash reads as a glitch. A short dissolve between photos or footage for a change of time or place is fine (`types/photo-slideshow.md`, `craft/editing.md`): a picture blends, text does not.

The source caps the end card at 1.4 s. Not in Mosio: the ending holds at least 2 s so the contact can be read and screenshotted.

## What good looks like

1. **The idea belongs to this brand or creator:** its real product or subject, how it is used or served, its materials, the place, their own phrases. Cover the logo: if it still fits the shop next door, the idea came from the category.
2. **The viewer sees something happen.** Claims are shown, not written: ice poured, a WA order arrives then gets packed, before and after, a step done on screen. One specific, real case from the user.
3. **Each beat causes the next.** Objects carry across cuts, motion direction stays consistent (see `mosio/video-composition/craft/motion.md`).
4. **Emphasis is rationed.** One hero per frame, at most three focal points; big needs small around it; at most a quarter of beats are "the point".
5. **Time breathes.** A short pause before what matters, enough reading time, stop before the idea runs out.
6. **One world.** One logic of light, material and color throughout; the background is a surface (gradient, light, shape, texture), not a swatch.
7. **One screenshot-worthy frame,** and it is the most worked-on moment.

Common client rejections: "too plain, just a picture then text", "lots of wasted space", "this part drags", "uneven spacing", "the opening is too dark" (food, health and household products: bright and clear).

## Truth

- Never invent prices, discounts, testimonials, customer names, ratings, units sold, followers, certifications, guarantees, addresses, hours, contacts, or facts in a lesson. All from the user; if missing, ask or leave it out.
- Every illustration carries a readable "Contoh" or "Ilustrasi" label: made-up chats, sample numbers, mock UI, sample reviews.
- Never copy the exact look or logo of another app (WhatsApp, Shopee, Tokopedia). A similar generic style is fine.
- From a reference video, only its structure, rhythm and kinds of motion: never its logo, text, characters, footage, music or a frame as is, and the reference file is not in the composition.
- On-screen claims must be true for this business ("dikirim hari ini" only if the user said so). Statements about the video too: never call it AI made. Mosio uses no AI models for images or video; the motion is written as code.

## Purpose on mute

Most people watch on mute: the sound effects are a bonus for those who listen, never the message. A first-time viewer, on mute and at phone size, must be able to answer at the end:

- **Selling (promo, ad, launch):** what is sold and by whom (name visible), one reason to care (taste, price, ease, result), what to do now (chat WA, visit, link in bio, checkout).
- **Teaching (explainer, tips):** the lesson in one sentence, the steps in order, who taught it and what to do next (save, try it, contact).
- **Launching:** what is new, why it matters, when and where to get it.
- **Telling a story:** the question, the answer, and what changed between them; whose story or channel it is.
- **A series episode:** this episode's one point, and which series it belongs to.
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
- A storyboard with motion words ("slams in", "wipe", "zoom") instead of roles and subjects. The user approves content; techniques come from the recipe plan.
- Facts or approved lines rewritten when only the recipe changed. Keep them; with the same kind of subject redo only the plan, with another kind re-fit only the subjects and say which beats changed.
- A second recipe built without being asked. Offer it in one line instead.

## Checklist every round

Mark each item **Pass** or **Fail**. A failed Critical item must be fixed before the user presses "Buat video".

| #   | Item                                                                                                                                                                                 | Level       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------- |
| 1   | Honest: no invented prices, testimonials, numbers, facts or contacts; illustrations labeled "Contoh"; claims that need proof have it (`platforms.md`)                                | Critical    |
| 2   | Purpose on mute is clear at the end (see above for the type); every object reads as what it is                                                                                       | Critical    |
| 3   | Ending fits the goal and the destination (`platforms.md`) held at least 2 s, in the safe zone; a loop ends in frame 0's state instead                                                | Critical    |
| 4   | Text inside the safe zone, at most 7 words at once, never cropped or colliding, including during transitions                                                                         | Critical    |
| 5   | Hook: frame 0 composed, a change by 0.5 s, the hook line read by 1 s; no empty frame longer than 3 frames, no white flash at cuts                                                    | Critical    |
| 6   | No flashing past the guide's limit (3 a second, no saturated red); colour is never the only cue. After a full preview or render, `get_status` lists any stretch over it as `flashes` | Critical    |
| 7   | None of the never-ship items, none of the defaults that look generated (`craft/motion.md`)                                                                                           | Important   |
| 8   | One hero per frame; with text covered, every scene still shows a picture                                                                                                             | Important   |
| 9   | Every seam has a cause and follows the vector law (axis, direction, speed, cut mid-motion); no crossfades between text scenes                                                        | Important   |
| 10  | Motion by role: entrances decelerate, exits accelerate; landed titles still; no 4 identical entrances; at most one visible spring                                                    | Important   |
| 11  | Timing: each line holds the guide's reading time; no fully frozen frame (background too) over 0.6 s except designed pauses and the ending                                            | Important   |
| 12  | Hold: the payoff lands late (after about 60%), the CTA after it; one re-hook in a video over about 15 s (`hooks.md`)                                                                 | Important   |
| 13  | Storyboard: roles and subjects, no motion words; facts and lines kept on a recipe change (subjects re-fitted only across kinds); no unasked recipe                                   | Important   |
| 14  | Sound: effects on the moments that matter and none over a spoken word that matters; music under speech 15-18 dB lower; nothing after the last hit                                    | Important   |
| 15  | One system: one motion personality, one accent color, at most two font families                                                                                                      | Opportunity |
| 16  | One screenshot-worthy frame, nameable by its second                                                                                                                                  | Opportunity |

Before the preview: run items 1-14 on the storyboard and code (tween positions, text sizes, colors, `data-start` of each sound). After it: run all 16, then compare with the last round, marking each old failure Fixed, Partial or Still there.

Write each failure for yourself as one line, `t=6.2s · where · problem → exact change · what stays`, so the fix is decided before you touch the code. Report briefly in the user's language, e.g. "Review putaran 2: 14 dari 16 lulus. Gagal: #5 (kilat putih di 6,2 s karena `#root` tanpa latar), #10 (lima kartu menu masuk dengan fade-up yang sama). Saya perbaiki dua itu dulu." Never a score out of 10: a score finds nothing to fix. Fix at most the three worst failures per round, biggest impact first: fixing everything at once hides which change broke something new. At most two inspection rounds before the preview (see "How to look"), fewer when the fixes turn cosmetic; the user's comments on the preview start the next round.

## Adapted from

- [Tejashmakwana/motionmaxxing](https://github.com/Tejashmakwana/motionmaxxing) (Apache-2.0, © Tejas Makwana), commit `8c8ec0f`: `SKILL.md`, `references/slop.md`.
- [echris6/motion-video-kit](https://github.com/echris6/motion-video-kit) (MIT, © echris6), commit `e4b1c00`: `business-motion-film/SKILL.md`, `business-motion-film/references/quality-bar.md`, `business-motion-film/references/critic-prompts.md`.
- [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) (MIT, © Kyle Zantos), commit `4a9ca87`: `skills/design-motion-principles/workflows/audit.md`, `skills/design-motion-principles/references/anti-checklist.md`, `skills/design-motion-principles/references/audit-checklist.md`.
- [howseen-ai/claude-motion-design](https://github.com/howseen-ai/claude-motion-design) (MIT, © Howseen AI (Raphaël Aubry)), commit `3d90d34`: `skill/motion-design/SKILL.md`.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes-creative/references/house-style.md` (copy in `skill_library`: `hyperframes/hyperframes-creative/references/house-style.md`).
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `b12365d`: `skills/hyperframes/references/review.md` (describe each frame as a stranger, naming every object; one-line notes `t · where · problem → change · what stays`; no score out of 10; cut a third of the motion when busy).

Changes by Mosio: translated, condensed and merged into one review routine for the preview flow (no audio pipeline, no render scripts, paused GSAP timeline). Dropped script-computed gates (look.py, lint.mjs, ffmpeg), separate sub-agent critics, NOTE.md and HTML report files, audio and loudness checks, cursor and 3D rules, and the 1.4 s end-card cap (replaced by Mosio's 2 s held ending). Added everyday business and creator examples, the "Contoh" label, a purpose-on-mute test per video type, and a scored checklist with Critical, Important and Opportunity levels, later extended with the hook stills, the flash limit, the payoff position and a sound item. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
