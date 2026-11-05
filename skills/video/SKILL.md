---
name: video
description: "Entry point for any Mosio video: promo, sale, paid ad, product launch, a story told in captions or in the user's own recorded voice, social content (tips, behind the scenes, series), education and explainers for a channel, app launch, announcement, chat story, photo slideshow, a short visual story, or an edit of the user's own footage (getting it in from the chat with an upload card, cut, trim, subtitles, zooms). Runs the flow from goal to brief and storyboard, picks the video type and the recipe to compose the HTML video from, then build, self-review and preview. Use whenever the user asks to make, plan or revise a video (bikin video, buat video promo, konten, iklan, video edukasi, video cerita, storytelling, video narasi, video launching, storyboard, ini videonya, upload video dari HP, edit video, potong video, kasih subtitle)."
license: "Proprietary; adapted parts are MIT or Apache-2.0, see NOTICE.md and Adapted from in each file"
metadata:
  author: Mosio
---

# Making a video with Mosio

Mosio renders short videos from HTML for small Indonesian businesses and creators, with sound effects from Mosio's bundled library (synthesized in code, and CC0 recordings) on the motion, plus the user's own footage or sound when they have it (`list_media`). You direct; Mosio checks, previews and renders. The hard rules live in `get_composition_guide` and win over anything here. Every look is a **recipe** (the dashboard's Recipes page): you write the HTML from the recipe's plan and example. `list_recipes` lists them with their ids; never hardcode them. Say "recipe" to the user; when the user says "template", they mean a recipe.

## Language

- These instructions are English. Reply in the user's language.
- On-screen text defaults to Indonesian for Indonesian businesses, otherwise it follows the user's language. Write it the way the owner talks to customers ("Pesan lewat WhatsApp", "Rp18.000", "Cek di Shopee").

## Flow

1. **Understand the goal.** What the video is for (sell, launch something new, tell a story, teach in one video or a channel's series, announce, edit the user's footage), for whom, where it is posted, and what the viewer should do or feel after watching. Take everything the user already said; do not ask again.
2. **Find who it speaks for.** `list_brands`; with several, ask which one unless the request already names it (a prompt from the dashboard carries `brand_id`). Read `get_brand`: voice, colours, fonts, logo, contact, products and prices. Never ask for what it already says, and pass the same `brand_id` to every tool that makes the video. A brand can be a business, a product line or a creator's channel. No brand: for a business video, offer to save one (`create_brand`); for a story, a lesson or a creator's post, go on without one (a name or handle set as text is enough).
3. **Pick the type** (table below) and read `types/<type>.md` (beside this file, or `skill_library` `mosio/video/types/<type>.md`). Route once: match the deliverable, not a word said in passing.
4. **Brief and concept** in chat, following `brief.md`: one batch of at most 5 questions, then the brief, then the look before the beats ("Concept"). A recipe brings its own world, kind of subject, text per beat and tempo, so beats written before it is known fit some recipes and break in others. A quick sale whose facts decide it needs no separate step; a story, a lesson or a launch gets 2-3 concepts that differ in idea, and the user picks one. A reference video the user shares is broken down first and shapes the concepts and the rhythm; it never goes on screen (`brief.md`, "A reference video").
5. **Storyboard for that look, and show it** with `preview_storyboard` (free): the beats (role, text, subject, weight; no motion) written for the chosen recipe, and in `recipes` that recipe, or for a quick sale 1-3 recipes of the same kind of subject (the card plays them as looks and offers the rest). A recipe the user already chose (a prompt from the dashboard's Recipes names it, e.g. "dengan recipe Produk utama (product-hero)") is the concept. Wait for content approval. A later switch to a recipe with another kind of subject keeps every fact and approved line but re-fits the subjects, with one more approval (`brief.md`, "Revisions").
6. **Build** a composition (below): `get_composition_guide` with the chosen `recipe`, then the HTML from the approved beats and that recipe's plan. The recipe gives the structure; the brand wins on colours, fonts, logo, words and motion feel (the guide's "Recipe and brand").
7. **Review** with `review.md` before the preview, and again after it.
8. **Preview.** `preview_composition`. It is free.
9. **Render.** The user watches the preview card and presses "Buat video" themselves, or "Buka di Studio" to fine-tune it by hand. Without a card (terminal, scripts), call `render_composition` only after the user agrees, then `get_status` until `done`, and share the download link and `project_url` (the video's page, where Studio opens it).
10. **Revise.** A change to a video that exists is a new version of the same video, never a new one: `get_project` (its HTML as it is now, with the user's Studio edits, and its `version`), change that HTML, then `preview_composition` and `render_composition` with `project_id` and `base_version`.

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
| Footage edit    | the user filmed something and wants it edited: a talking head, a testimonial, a product or process montage, a shop tour, a short cut from a long recording | `types/footage-edit.md`    |
| Visual story    | a narrative "why / how" explainer that feels like a short film: one illustrated world, a moving camera, word-by-word captions, a computed number, a payoff | `types/visual-story.md`    |

Ambiguous cases:

- A promo posted as paid reach is an Ad. The same promo posted organically is a Promo.
- "Tips" or "cara" that also sells a product: Explainer for structure, with the product as the example and the CTA at the end.
- Explainer or Visual story: steps, how-to and tips are an Explainer; one "why" or "how come" question answered like a short film, with a number and a payoff, is a Visual story.
- A customer review: Chat story when told as a conversation, a Footage edit when the customer is on video, otherwise Promo.
- Their own clips: Footage edit when the ask is the edit itself ("potong ini", "kasih subtitle", "jadikan Reels"). A promo, ad or tips video built on their clip follows the type that matches the goal and uses `types/footage-edit.md` and the editing craft for the clip work. Photos only: Photo slideshow.
- More than one type fits: pick the one whose success action matches the user's goal, say why in one line.

## Pick the recipe and build

- The recipes, with what each is for, come from `list_recipes` or `get_composition_guide`; `types/<type>.md` names the nearest one per content kind. A recipe brings a plan, art direction and example code; a library recipe (`library: true`) only a style's art direction.
- Build with the `composition` skill: `get_composition_guide` with the chosen `recipe`, then `skill_library` `mosio/video-composition/craft/` (motion, transitions, kinetic type, captions, camera, numbers, logo, sizes; for the user's footage: editing, reframe, zooms, subtitles; text effects). Tools: `inspect_composition`, `preview_composition`, `render_composition`.
- The recipe's plan and example are a reference, never a mold: change text, color, rhythm, shapes and scenes to fit the brand and the storyboard.
- Unsure which the user wants, or they ask what exists: `show_recipes`. Do not list recipes in chat when a card can show them.
- For a ready effect (kinetic text, transition, overlay, UI card, chart) or a whole scene (lower third, scene transition, code or chart scene), and for word-timed captions or animated icons, try `search_registry` (English keywords) and `get_registry_item` before hand-building it. Blocks are whole scenes, mostly 1920×1080: use them as they are in a landscape video only.

## Composition essentials

- The guide's technical rules are checked before render: one HTML file, one paused GSAP timeline, no clock, randomness or CDN, the user's own photos, footage and sound only at the paths `list_media` gives, small `data:` images otherwise, under 200,000 characters, CPU render.
- Every type keeps: the guide's sizes and safe zones, at most 7 words on screen at once, an ending (CTA, name, contact) held at least 2 s.
- A third-party skill in `skill_library` that says to run a CLI, use media or another framework: follow the note in the library index. Mosio's rules win.

## Truth

- Never invent prices, discounts, testimonials, ratings, numbers, contacts, addresses or opening hours. Ask, or leave them out of the video.
- What the user says in this conversation wins over the brand (its prices may be out of date, or this is a promo). Use the user's facts, mention the difference in one line in the brief ("di brand tercatat Rp18.000; saya pakai Rp25.000 dari kakak"), and keep going: never hold the storyboard for it.
- Label every illustration on screen: "Contoh" or "Ilustrasi" (sample chats, sample numbers, mock UI).
- Never call Mosio an "AI video generator". Mosio uses no AI models to make images or video; the motion is written as code (a voice-over is spoken with the user's own Gemini or ElevenLabs key).

## Credits

Each render costs credits equal to its duration, and each size is its own render (a 15 s video in 9:16 and 1:1 costs 30 s). Previews are free. Offer before making several sizes or versions; never make them unasked. Failed or cancelled videos cost nothing. Out of credits (`insufficient_credits`): say so plainly (the balance and what the video needs are in `details`; credits refill at the start of the next cycle) and keep the free preview. Do not push a plan or a purchase.

## Errors

- No Mosio tools in this conversation (not connected or not signed in): ask the user to connect Mosio and sign in, then stop. Never claim a storyboard, preview or video exists.
- `preview_composition` rejected: read `details` (lint code, message, fix), fix, send again.
- `too_many_active_renders`: wait for the running video. Wrong video still rendering: `cancel_video`, then redo.

## Adapted from

- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (Apache-2.0, © HeyGen and HyperFrames contributors), commit `908f2d0`: `skills/hyperframes/SKILL.md`.

Changes by Mosio: kept only the routing idea (one entry point, route once by the requested deliverable, first matching row, explicit tie-breakers) and rewrote it for Mosio's video types and its composition path, with Mosio's composition rules (paused GSAP timeline, no audio, no CDN). Dropped project state, CLI install and usage checks, media, audio and Studio routing. Added the flow (with the brand step and recipes), the language, truth and credit rules. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
