---
name: video-composition
description: "Write a video as an HTML composition that Mosio renders: the technical rules, sizes and safe zones, fonts, motion language, 10 example styles, and craft references for motion, transitions, kinetic type, captions, the 2D camera, numbers, logos and multiple sizes. Use for any custom video, after the brief and storyboard are approved."
license: "Proprietary; craft/ adapts MIT and Apache-2.0 works, see NOTICE.md"
metadata:
  brand: Mosio
---

# Mosio video composition

You design the video yourself as one HTML file (a Hyperframes composition): layout, typography, colour and motion. Mosio checks it, shows a moving preview in the card, and renders it to MP4 on its servers. You are the director; Mosio is the studio and the camera.

## Flow
1. Understand the goal: what it is for (sell, teach, announce), who it is for, and where it will be posted (see Sizes; vertical 1080×1920 by default). Never invent prices, addresses, contacts, opening hours or numbers; ask when they are missing.
2. **Brief and storyboard first, before any HTML.** Write them in the chat, short:
   - Brief: the subject or offer, the audience, the call to action, the contact, the size(s) and the duration.
   - Storyboard: 3-6 scenes, each with its time range, on-screen text, and motion.

   Ask whether it is right and revise at this stage (free and fast). Start the HTML once the user agrees, unless they asked for it to be made straight away. More: `skill_library` `mosio/video/brief.md`.
3. Pick a fitting style and read its example code (`get_composition_guide` with `style`). The example is a starting point, not a mold: change the text, colours, rhythm, shapes and scenes to fit the user and the approved storyboard.
4. Write the full HTML and call `preview_composition`. If it is rejected, read `details` (lint code, message, suggested fix), fix it, and send it again.
5. The user watches the preview in the card and presses "Buat video" themselves (this spends credits equal to the duration). Revise through the chat: change the HTML and call `preview_composition` again.
6. In apps without cards (a terminal, scripts): once the user agrees, call `render_composition`, then `get_status` until `done`.

## Sizes
| Format | `data-width` × `data-height` | For |
| --- | --- | --- |
| Vertical 9:16 (default) | 1080 × 1920 | Reels, TikTok, Shorts, WhatsApp Status |
| Portrait 4:5 | 1080 × 1350 | Instagram feed, ads |
| Square 1:1 | 1080 × 1080 | Instagram and Facebook feeds, marketplaces |
| Wide 16:9 | 1920 × 1080 | YouTube, websites, in-store screens |

- One brief can become several sizes: write one composition per size with the same scene order and duration. Re-lay each size out (e.g. text beside the visual at 16:9); do not just scale or crop.
- Each size renders separately and spends credits equal to its duration. Offer before making several sizes at once.
- Videos render without sound. If the user wants music, suggest adding it in the app they post to (Reels, TikTok).

## Language
On-screen text follows the user's language: Indonesian by default for Indonesian businesses (prices as `Rp25.000`). Reply to the user in their language.

## Director's library
`skill_library` starts with Mosio's own skills, which already follow the rules below: `mosio/video/` (the flow, brief, review, and video types: promo, ad, visual story, social content, explainer, app launch, chat story, photo slideshow) and `mosio/video-composition/craft/` (motion, transitions, kinetic type, captions, camera, numbers, logo, sizes). It also holds third-party skills you can read: official Hyperframes (the composition contract `hyperframes/hyperframes-core/SKILL.md`, motion rules, scene blueprints and transitions in `hyperframes/hyperframes-animation/`, creative direction in `hyperframes/hyperframes-creative/`), motion design, short-form structure, kinetic typography, GSAP, and copywriting. For ready-made effects (kinetic text, transitions, overlays, UI cards, charts), search the Hyperframes registry with `search_registry` (English keywords), fetch with `get_registry_item`, and paste it into your composition instead of starting from scratch. When a third-party skill says to run a CLI, use media, or another framework, follow the notes in the library index. The rules below are what apply on Mosio.

## Technical rules (required, checked before render)
- One HTML document. Root element: `<div id="root" data-composition-id="main" data-start="0" data-duration="15" data-width="1080" data-height="1920" data-fps="30">`. Suggested duration 6-30 seconds, at most 60 seconds. `html` and `body` are `data-width` × `data-height` (e.g. 1080×1920 px), and so is `<meta name="viewport">`; `#root` is `width: 100%; height: 100%; overflow: hidden` (the canvas size comes from `data-width`/`data-height`; do not hard-code it on `#root`).
- All motion lives on one paused, registered GSAP timeline; the renderer plays it frame by frame:
  ```js
  const tl = gsap.timeline({ paused: true });
  // ... tweens at explicit times: tl.from("#title", {...}, 0.4)
  window.__timelines = window.__timelines || {};
  window.__timelines["main"] = tl;
  ```
- Not allowed: `setTimeout`, `setInterval`, `requestAnimationFrame`, CSS animations or transitions, `Date.now()`, `new Date()`, `Math.random()`. None of them follows the timeline, so the render would differ from the preview. Need randomness? Use a small seeded PRNG (mulberry32). Need a loop? `repeat` with a fixed number, never `-1`.
- Assets: `<script src="vendor/gsap.min.js"></script>` (GSAP 3.14), GSAP plugins as `vendor/<Name>.min.js` (CustomEase, CustomBounce, CustomWiggle, DrawSVGPlugin, EasePack, Flip, MorphSVGPlugin, MotionPathPlugin, Physics2DPlugin, PhysicsPropsPlugin, ScrambleTextPlugin, SplitText, TextPlugin; register them with `gsap.registerPlugin`), `vendor/mosio.js` (optional: `Mosio.readableOn(hex)` returns a black or white text colour that reads on it), the fonts below, inline SVG and CSS, and small `data:` images. GSAP CDN URLs (jsdelivr, cdnjs, unpkg) are swapped for the local copy automatically. Otherwise no http(s) URLs, iframes, video or audio: the render runs offline.
- Write text straight into the HTML. Escape `&` and `<` in text. No `<br>` in body text (let it wrap with `max-width`); the only exception is a short headline deliberately set one word per line.
- Do not give an element an initial CSS `transform` on a property GSAP tweens; set initial states on the timeline (`fromTo`, `xPercent`/`yPercent`).
- Renders use the CPU: avoid WebGL, large `filter: blur` on full-screen elements, `backdrop-filter`, and hundreds of moving elements. Gradients, SVG, transforms, opacity and clip-path are cheap.
- Move things with transforms (`x`, `y`, `xPercent`, `scale`, `rotation`) and `opacity`, not `top`/`left`/`width`.

## Fonts
Two options, identical in preview and render:
- **Google Fonts**: link them as usual, e.g. `<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Anton&family=Inter:wght@400;800&display=swap">`. Mosio downloads the Latin glyphs while checking and stores them with the project. Pick families that exist on Google Fonts.
- **Bundled fonts** below, with no download. Declare the ones you use:
```css
@font-face { font-family: "Bebas Neue"; src: url("vendor/BebasNeue-400.woff2"); font-weight: 400; }
```

| Family | Weight | File | Feel |
| --- | --- | --- | --- |
| Bebas Neue | 400 | `vendor/BebasNeue-400.woff2` | tall condensed headlines, kinetic type, sport, promos |
| Archivo Black | 400 | `vendor/ArchivoBlack-400.woff2` | heavy and loud: discounts, prices, big numbers |
| Poppins | 500 | `vendor/Poppins-500.woff2` | friendly, modern body text |
| Poppins | 800 | `vendor/Poppins-800.woff2` | modern geometric headlines |
| Playfair Display | 700 | `vendor/PlayfairDisplay-700.woff2` | elegant: cafes, cakes, beauty, boutiques |
| Playfair Display | 400 italic | `vendor/PlayfairDisplay-400-italic.woff2` | elegant italic accents |
| Space Grotesk | 500 | `vendor/SpaceGrotesk-500.woff2` | tech, gadgets, digital services |
| Space Grotesk | 700 | `vendor/SpaceGrotesk-700.woff2` | tech headlines |
| Caveat | 700 | `vendor/Caveat-700.woff2` | handwriting: homemade, notes, a personal touch |
| Noto Sans | 400 | `vendor/NotoSans-Regular.ttf` | neutral, a fallback for any text |
| Noto Sans | 700 | `vendor/NotoSans-Bold.ttf` | neutral bold |

Use at most two families per video: one for headlines, one for body text.

## Layout and safe zones
- Vertical 9:16: keep important text out of the top 250 px, the bottom 420 px (Reels/TikTok buttons and captions), and 80 px on the left and right.
- Portrait 4:5 and square 1:1: keep 80 px clear on every edge.
- Wide 16:9: keep 120 px clear left and right and 80 px top and bottom; text spans at most about half the screen, the rest is for visuals.
- Headlines 120-260 px, supporting text 54-80 px, nothing smaller than 40 px. At most 7 words on screen at once.
- Enough contrast: for text on a brand colour use `Mosio.readableOn`, or pick a colour that clearly reads.

## Motion language (what makes it feel current)
- A 15-second structure: an opening at 0-1.5 s that grabs at once (big motion, short text), 2-4 body scenes (one idea per scene, 2-4 s each), and an ending with the call to action, the name and the contact held still for at least 2 seconds.
- Change scenes with a transition: a wipe or mask (`clipPath`), a slide, a zoom, or words leaving before new words enter. Never just appear and disappear.
- Easing: enter with `expo.out` or `power3.out`, exit with `power2.in`, bounce with `back.out(1.6)` sparingly. Moves last 0.35-0.8 s. Stagger words or letters by 0.03-0.08 s.
- Masked reveals: wrap text in `overflow: hidden`, then `from(span, { yPercent: 110 })`. The hallmark of tidy motion.
- The background is always a little alive (a slowly drifting gradient or shape, a moving line), but text being read holds still for at least 1.2 seconds.
- Counting numbers: tween an object `{ v: 0 }` with an `onUpdate` that writes formatted text (e.g. `Rp${Math.round(o.v).toLocaleString("id-ID")}`).
- Colour: the brand's main colour, the background colour, and one accent. Derive shades with `color-mix()`.
- Finish cleanly: hold the ending, then fade or scale out in the last 0.5 seconds.
- More craft: `mosio/video-composition/craft/` in `skill_library`.

## Before sending
- All text fits and reads inside the safe zones; nothing is cut off.
- Every element that appears later gets its initial state from `from`/`fromTo` on the timeline, not hidden with CSS.
- The timeline length equals `data-duration`.

## Styles
Read the example before writing; treat it as a starting point, not a mold.

- [kinetic-type](styles/kinetic-type.html): Big words take turns on a rhythm. For slogans, calls to action and announcements.
- [product-hero](styles/product-hero.html): One product shown big with light, its name, and a price that counts up. For selling one product.
- [flash-sale](styles/flash-sale.html): A bursting discount percent, a running ribbon, and a static deadline. For time-limited sales.
- [menu-board](styles/menu-board.html): Menu items and prices appear in turn like a cafe menu board. For food and drink businesses.
- [chat-testimoni](styles/chat-testimoni.html): A customer review typed out as chat bubbles, then stars and the business name.
- [angka-capaian](styles/angka-capaian.html): Numbers count up and bar charts grow. For milestones, comparisons and social proof.
- [peluncuran-aplikasi](styles/peluncuran-aplikasi.html): The problem is struck out, the app window slides in, a cursor clicks, and numbers rise. For apps, SaaS and digital products.
- [dasbor-iklan](styles/dasbor-iklan.html): Metrics drop then rise, platform cards, and a chart line drawing itself. For ad services, agencies and digital services.
- [explainer-isometrik](styles/explainer-isometrik.html): An isometric stage: the problem, the workload, then the solution and a chart. A 1920×1080 wide example for YouTube, websites and in-store screens.
- [visual-story](styles/visual-story.html): A short explainer that feels like a film: one illustrated 2D world under a moving camera, word-by-word captions with a selection box on key words, computed numbers. For education and 'why' facts.

## Craft
- [camera.md](craft/camera.md): Camera: one 2D world
- [captions.md](craft/captions.md): Captions: the word-by-word engine
- [kinetic-type.md](craft/kinetic-type.md): Kinetic type
- [logo.md](craft/logo.md): Logo and end card
- [motion.md](craft/motion.md): Motion craft
- [numbers.md](craft/numbers.md): Numbers and charts
- [sizes.md](craft/sizes.md): One brief, several sizes
- [transitions.md](craft/transitions.md): Transitions
