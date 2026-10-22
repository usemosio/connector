---
name: video
description: "Entry point for any Mosio video: promo, sale, paid ad, social content (tips, behind the scenes, series), education and explainers, app launch, announcement, chat story, photo slideshow, or a short visual story. Runs the flow from goal to brief and storyboard, picks the video type and the recipe (a fill-in recipe with a fixed design, or one to compose an HTML video from), then build, self-review and preview. Use whenever the user asks to make, plan or revise a video (bikin video, buat video promo, konten, iklan, video edukasi, storyboard)."
license: "Proprietary; adapted parts are MIT or Apache-2.0, see NOTICE.md and Adapted from in each file"
metadata:
  author: Mosio
---

# Making a video with Mosio

Mosio renders short, silent videos from HTML for small Indonesian businesses and creators. You direct; Mosio checks, previews and renders. The hard rules live in `get_composition_guide` and win over anything here. Every look is a **recipe** (the dashboard's Recipes page), in two kinds: recipes you compose from (you write the HTML from the recipe's plan and example) and fill-in recipes (fixed designs where only the text changes). `list_recipes` lists both with their ids; never hardcode them. Say "recipe" to the user; when the user says "template", they mean a fill-in recipe.

## Language

- These instructions are English. Reply in the user's language.
- On-screen text defaults to Indonesian for Indonesian businesses, otherwise it follows the user's language. Write it the way the owner talks to customers ("Pesan lewat WhatsApp", "Rp18.000", "Cek di Shopee").

## Flow

1. **Find the brand.** `list_brands`; with several, ask which one unless the request already names it (a prompt from the dashboard carries `brand_id`). Read `get_brand`: voice, colours, fonts, logo, contact, products and prices. Never ask for what it already says, and pass the same `brand_id` to every tool that makes the video.
2. **Understand the goal.** What is sold, taught or announced, for whom, where it is posted, and the one action after watching. Take everything the user already said; do not ask again.
3. **Pick the type** (table below) and read `types/<type>.md` (beside this file, or `skill_library` `mosio/video/types/<type>.md`). Route once: match the deliverable, not a word said in passing.
4. **Brief and content storyboard** in chat, following `brief.md`: one batch of at most 5 questions, then the brief and the beats (role, text, subject, weight; no motion).
5. **Show it** with `preview_storyboard` (free) in 1-3 fitting recipes, or only the recipe the user already chose (a prompt from the dashboard's Recipes names it, e.g. "dengan recipe Produk utama (product-hero)"). Wait for content approval, then the user picks a recipe. A later recipe change does not reopen the content.
6. **Pick the path**: a fill-in recipe or a composition (below).
7. **Build.** Fill-in recipe: fill its variables. Composition: `get_composition_guide` with the chosen `recipe`, then the HTML from the approved beats and that recipe's plan. The recipe gives the structure; the brand wins on colours, fonts, logo, words and motion feel (the guide's "Recipe and brand").
8. **Review** with `review.md` before the preview, and again after it.
9. **Preview.** `preview_recipe` or `preview_composition`. Both are free.
10. **Render.** The user watches the preview card and presses "Buat video" themselves, or "Buka di Studio" to fine-tune it by hand. Without a card (terminal, scripts), call `render_recipe` or `render_composition` only after the user agrees, then `get_status` until `done`, and share the download link and `project_url` (the video's page, where Studio opens it).
11. **Revise.** A change to a video that exists is a new version of the same video, never a new one: `get_project` (its HTML as it is now, with the user's Studio edits, and its `version`), change that HTML, then `preview_composition` and `render_composition` with `project_id` and `base_version`.

"Langsung buat aja" or "yang cepet": skip waiting for the storyboard approval and go straight to a preview in the recipe the user chose, or the best-fitting one, but still write a short brief and the beats as decisions and never guess facts. The render still waits for the user: the preview is free, the render spends credits.

## Pick the type

| Type            | When                                                                                                                                                       | File                       |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| Promo           | sale, discount, strikethrough price, one product with its price, menu, service, store opening, announcement                                                | `types/promo.md`           |
| Ad              | paid Meta or TikTok ads, hook and offer tested for cold audiences                                                                                          | `types/ad.md`              |
| Social content  | regular posts: tips, behind the scenes, series, trends; built to be watched to the end                                                                     | `types/social-content.md`  |
| Explainer       | education: how it works, steps, myth vs fact, before and after; must teach on mute                                                                         | `types/explainer.md`       |
| App launch      | app, SaaS or digital product launch, UI demo                                                                                                               | `types/app-launch.md`      |
| Chat story      | a story or testimonial told as chat bubbles                                                                                                                | `types/chat-story.md`      |
| Photo slideshow | the user has product photos to show                                                                                                                        | `types/photo-slideshow.md` |
| Visual story    | a narrative "why / how" explainer that feels like a short film: one illustrated world, a moving camera, word-by-word captions, a computed number, a payoff | `types/visual-story.md`    |

Ambiguous cases:

- A promo posted as paid reach is an Ad. The same promo posted organically is a Promo.
- "Tips" or "cara" that also sells a product: Explainer for structure, with the product as the example and the CTA at the end.
- Explainer or Visual story: steps, how-to and tips are an Explainer; one "why" or "how come" question answered like a short film, with a number and a payoff, is a Visual story.
- A customer review: Chat story when told as a conversation, otherwise Promo (fill-in recipe `testimoni`).
- More than one type fits: pick the one whose success action matches the user's goal, say why in one line.

## Pick the path

|      | Fill-in recipe (fast)                                                                                                                                                                                                                  | Composition (default)                                                                                                                                                                                                                                                  |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| When | a fill-in recipe fits and the user only needs their text swapped, or picked one in the gallery, or wants it quick ("pakai template aja")                                                                                               | anything unique, anything that follows the storyboard, any type without a fill-in recipe                                                                                                                                                                               |
| How  | the "Fill-in recipes" section of the server instructions (also `skill_library` `mosio/video/fill-in.md`). Tools: `list_recipes` (`kind: "fill_in"`: variables and limits), `show_recipes` (gallery), `preview_recipe`, `render_recipe` | the `video-composition` skill: `get_composition_guide` with the chosen `recipe`, then `skill_library` `mosio/video-composition/craft/` (motion, transitions, kinetic type, captions, camera, numbers, logo, sizes). Tools: `preview_composition`, `render_composition` |
| Look | the recipe's fixed design                                                                                                                                                                                                              | the chosen recipe's plan and example (`get_composition_guide` with `recipe`), or a fill-in recipe as a reference, never a mold: change text, color, rhythm, shapes and scenes to fit the brand and the storyboard                                                      |

- Fill-in recipes exist only for promo-type content today (products, discounts, menus, services, reviews, openings, announcements; `list_recipes` has the current ids). Social content, explainers, ads, app launches, chat stories and slideshows go through composition.
- The recipes to compose from, with what each is for, come from `list_recipes` or `get_composition_guide`; `types/<type>.md` names the nearest one per content kind.
- Unsure which the user wants, or they ask what exists: `show_recipes`. Do not list recipes in chat when a card can show them.
- For a ready effect (kinetic text, transition, overlay, UI card, chart), try `search_registry` (English keywords) and `get_registry_item` before hand-building it.

## Composition essentials

- The guide's technical rules are checked before render: one HTML file, one paused GSAP timeline, no clock, randomness, audio, video or CDN, small `data:` images only, under 200,000 characters, CPU render.
- Every type keeps: the guide's sizes and safe zones, at most 7 words on screen at once, an ending (CTA, name, contact) held at least 2 s.
- A third-party skill in `skill_library` that says to run a CLI, use media or another framework: follow the note in the library index. Mosio's rules win.

## Truth

- Never invent prices, discounts, testimonials, ratings, numbers, contacts, addresses or opening hours. Ask, or leave them out of the video.
- What the user says in this conversation wins over the brand (its prices may be out of date, or this is a promo). Use the user's facts, mention the difference in one line in the brief ("di brand tercatat Rp18.000; saya pakai Rp25.000 dari kakak"), and keep going: never hold the storyboard for it.
- Label every illustration on screen: "Contoh" or "Ilustrasi" (sample chats, sample numbers, mock UI).
- Never call Mosio an "AI video generator". Mosio uses no AI models to make images, video or audio; the motion is written as code.

## Credits

Each render costs credits equal to its duration, and each size is its own render (a 15 s video in 9:16 and 1:1 costs 30 s). Previews are free. Offer before making several sizes or versions; never make them unasked. Failed or cancelled videos cost nothing. Out of credits (`insufficient_credits`): suggest topping up in the Mosio dashboard.

## Errors

- `preview_composition` or `preview_recipe` rejected: read `details` (lint code, message, fix), fix, send again.
- `invalid_variables`: fix the named variable, retry with a new `idempotency_key`.
- `too_many_active_renders`: wait for the running video. Wrong video still rendering: `cancel_video`, then redo.

## Adapted from

- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes/SKILL.md`.

Changes by Mosio: kept only the routing idea (one entry point, route once by the requested deliverable, first matching row, explicit tie-breakers) and rewrote it for Mosio's video types and its two paths, template or composition, with Mosio's composition rules (paused GSAP timeline, no audio, no CDN). Dropped project state, CLI install and usage checks, media, audio and Studio routing. Added the flow (with the brand step and recipes), the language, truth and credit rules. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
