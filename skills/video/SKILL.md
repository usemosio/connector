---
name: video
description: "Entry point for any Mosio video: promo, sale, paid ad, social content (tips, behind the scenes, series), education and explainers, app launch, announcement, chat story, or photo slideshow. Runs the flow from goal to brief and storyboard, picks the video type and the path (a ready Mosio template or a custom HTML composition), then build, self-review and preview. Use whenever the user asks to make, plan or revise a video (bikin video, buat video promo, konten, iklan, video edukasi, storyboard)."
license: "MIT and Apache-2.0; see Adapted from in each file"
metadata:
  author: Mosio
  version: "0.2.0"
---

# Making a video with Mosio

Mosio renders short, silent videos from HTML for small Indonesian businesses and creators. You direct; Mosio checks, previews and renders. The hard rules live in `get_composition_guide` and win over anything here.

## Language

- These instructions are English. Reply in the user's language.
- On-screen text defaults to Indonesian for Indonesian businesses, otherwise it follows the user's language. Write it the way the owner talks to customers ("Pesan lewat WhatsApp", "Rp18.000", "Cek di Shopee").

## Flow

1. **Understand the goal.** What is sold, taught or announced, for whom, where it is posted, and the one action after watching. Take everything the user already said; do not ask again.
2. **Pick the type** (table below) and read `types/<type>.md`. Route once: match the deliverable, not a word said in passing.
3. **Brief and storyboard** in chat, following `brief.md`: one batch of at most 5 questions, then the brief and a second-by-second storyboard. Wait for approval. Revisions here are free.
4. **Pick the path**: template or custom composition (below).
5. **Build.** Template: fill its variables. Composition: write the HTML.
6. **Review** with `review.md` before the preview, and again after it.
7. **Preview.** `preview_template` or `preview_composition`. Both are free.
8. **Render.** The user watches the preview card and presses "Buat video" themselves. Without a card (terminal, scripts), call `render_template` or `render_composition` only after the user agrees, then `get_status` until `done`.

"Langsung buat aja" or "yang cepet": skip waiting for the storyboard approval and go straight to a preview, but still write a short brief and storyboard as decisions and never guess facts. The render still waits for the user: the preview is free, the render spends credits.

## Pick the type

| Type            | When                                                                                                        | File                       |
| --------------- | ----------------------------------------------------------------------------------------------------------- | -------------------------- |
| Promo           | sale, discount, strikethrough price, one product with its price, menu, service, store opening, announcement | `types/promo.md`           |
| Ad              | paid Meta or TikTok ads, hook and offer tested for cold audiences                                           | `types/ad.md`              |
| Social content  | regular posts: tips, behind the scenes, series, trends; built to be watched to the end                      | `types/social-content.md`  |
| Explainer       | education: how it works, steps, myth vs fact, before and after; must teach on mute                          | `types/explainer.md`       |
| App launch      | app, SaaS or digital product launch, UI demo                                                                | `types/app-launch.md`      |
| Chat story      | a story or testimonial told as chat bubbles                                                                 | `types/chat-story.md`      |
| Photo slideshow | the user has product photos to show                                                                         | `types/photo-slideshow.md` |

Ambiguous cases:

- A promo posted as paid reach is an Ad. The same promo posted organically is a Promo.
- "Tips" or "cara" that also sells a product: Explainer for structure, with the product as the example and the CTA at the end.
- A customer review: Chat story when told as a conversation, otherwise Promo (template `testimoni`).
- More than one type fits: pick the one whose success action matches the user's goal, say why in one line.

## Pick the path

|      | Template (fast)                                                                                                                                                                             | Custom composition (default)                                                                                                                                                                             |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| When | an existing Mosio template fits and the user only needs their text swapped, or picked a template in the gallery, or wants it quick                                                          | anything unique, anything that follows the storyboard, any type without a template                                                                                                                       |
| How  | the `video-template` skill (in `skill_library`: the server instructions). Tools: `list_templates` (variables and limits), `show_templates` (gallery), `preview_template`, `render_template` | the `video-composition` skill: `get_composition_guide`, then `../video-composition/craft/` (motion, transitions, kinetic type, numbers, logo, sizes). Tools: `preview_composition`, `render_composition` |
| Look | the template's design                                                                                                                                                                       | the closest style (`get_composition_guide` with `style`) or template as a reference, never a mold: change text, color, rhythm, shapes and scenes to fit                                                  |

- Templates exist only for promo-type content today: `promo-produk`, `diskon-kilat`, `menu-fnb`, `menu-spesial`, `jasa-layanan`, `testimoni`, `buka-baru`, `pengumuman`. Social content, explainers, ads, app launches, chat stories and slideshows go through composition.
- Style ids for reference: `kinetic-type`, `product-hero`, `flash-sale` (static deadline text, no countdown), `menu-board`, `chat-testimoni`, `angka-capaian`, `peluncuran-aplikasi`, `dasbor-iklan`, `explainer-isometrik`.
- Unsure which the user wants, or they ask what exists: `show_templates`. Do not list templates in chat when a card can show them.
- For a ready effect (kinetic text, transition, overlay, UI card, chart), try `search_registry` (English keywords) and `get_registry_item` before hand-building it.

## Composition essentials

- The guide's technical rules are checked before render: one HTML file, one paused GSAP timeline, no clock, randomness, audio, video or CDN, small `data:` images only, under 200,000 characters, CPU render.
- Every type keeps: the guide's sizes and safe zones, at most 7 words on screen at once, an ending (CTA, name, contact) held at least 2 s.
- A third-party skill in `skill_library` that says to run a CLI, use media or another framework: follow the note in the library index. Mosio's rules win.

## Truth

- Never invent prices, discounts, testimonials, ratings, numbers, contacts, addresses or opening hours. Ask, or leave them out of the video.
- Label every illustration on screen: "Contoh" or "Ilustrasi" (sample chats, sample numbers, mock UI).
- Never call Mosio an "AI video generator". Mosio uses no AI models to make images, video or audio; the motion is written as code.

## Credits

Each render costs credits equal to its duration, and each size is its own render (a 15 s video in 9:16 and 1:1 costs 30 s). Previews are free. Offer before making several sizes or versions; never make them unasked. Failed or cancelled videos cost nothing. Out of credits (`insufficient_credits`): suggest topping up in the Mosio dashboard.

## Errors

- `preview_composition` rejected: read `details` (lint code, message, fix), fix, send again.
- `invalid_variables`: fix the named variable, retry with a new `idempotency_key`.
- `too_many_active_renders`: wait for the running video. Wrong video still rendering: `cancel_video`, then redo.

## Adapted from

- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes/SKILL.md`.

Changes by Mosio: kept only the routing idea (one entry point, route once by the requested deliverable, first matching row, explicit tie-breakers) and rewrote it for Mosio's video types and its two paths, template or composition, with Mosio's composition rules (paused GSAP timeline, no audio, no CDN). Dropped project state, CLI install and usage checks, media, audio and Studio routing. Added the flow, the language, truth and credit rules. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
