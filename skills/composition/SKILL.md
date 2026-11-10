---
name: composition
description: "Write a video as an HTML composition that Mosio renders: one file, one paused GSAP timeline, the brand's colours and fonts, a recipe's plan and example. Use for any custom motion video after the brief and storyboard are approved, to change an existing video, or when the user shares HTML to turn into a video. The rules come from the Mosio server."
license: Proprietary
metadata:
  brand: Mosio
---

# Mosio video composition

You design the video yourself as one HTML file (a Hyperframes composition): layout, typography, colour and motion. Mosio checks it, shows a moving preview in the card, and renders it to MP4 on its servers. The looks you build from are **recipes** (92 today; `list_recipes` has the current list). Say "recipe" to the user.

Everything below is read from the Mosio server when you need it, so it is always current. Do not work from memory of an earlier version.

## Flow

1. Follow the `video` skill first: goal, brand (`list_brands`, `get_brand`), type, brief, the concept (the recipe), the content storyboard written for it, `preview_storyboard` and the user's approval.
2. Call `get_composition_guide` with the `brand_id` and the chosen `recipe`. It returns the technical rules (root element, sizes and safe zones, the one paused GSAP timeline, what is not allowed, assets and fonts, sound effects), the brand brief and its colour and font tokens, a sound kit for the brand's motion style, the recipe's plan (a technique per beat role and seconds per weight), its art direction and its example code. The rules in it win over anything you remember.
3. For craft beyond the guide, the `motion` skill: `skill_library` `mosio/video-composition/craft/` (motion, transitions, kinetic type, captions, camera, numbers, logo, sizes, sound; for the user's footage: editing, reframe, zooms, subtitles; text effects) and, for a ready-made effect, `search_registry` then `get_registry_item`.
4. Write the full HTML from the approved beats and the recipe's plan, with sound effects from the kit on the main moments (the guide's Sound effects). The recipe gives the structure; the brand wins on colours, fonts, logo, words and motion feel. Change the example's text, colours, scenes and rhythm; never send it as is.
5. Look before the user does (the `review` skill): `inspect_composition` with the same `html` and `at` at the middle of each beat returns one contact sheet and measured findings with fix hints. Fix, or mark an effect you meant with `data-layout-allow-overflow` / `data-layout-allow-overlap`, and inspect again, at most twice.
6. **On a machine with a shell** (Claude Code, Codex, Cursor): `get_composition_bundle` packs the composition exactly as the render would and returns commands to check it locally with `mosio-frames` (the same stills and layout checks as `inspect_composition`, faster, no server queue) and, on plans that allow it, to render the MP4 locally without credits and attach it to the video. Run them only with the user's approval. The final video for posting can always be rendered on the server.
7. **Fast preview**, every time: `preview_composition` (free, instant) with the inspected HTML. If it is rejected, read `details.problems` (lint code, message, fix), fix, and send again. **Full preview**, only offered: `render_composition` with `quality: "draft"` makes the real MP4, faster and lower quality, and costs credits like a render. Offer it only when step 6 is not available and the client shows no card, when footage or a voice-over must be checked in sync, or when the user wants the real file first; say the cost before. A draft is never for posting.
8. The user watches the preview in the card and presses "Buat video" themselves (credits equal to the duration), or "Buka di Studio". In apps without cards, call `render_composition` (`final`, the same `project_id` as a draft) only after the user agrees, then `get_status`, and share the download link and `project_url`. Save the `project_id` into the production plan.
9. A change to an existing video is a new version of the same video: `get_project` (current HTML, with the user's Studio edits, and its `version`), edit that HTML, keep element ids, then `preview_composition` and `render_composition` with `project_id` and `base_version`.

## Always

- One HTML file, one paused GSAP timeline registered as `window.__timelines["main"]`, no clock, randomness, CSS animations, iframes or network; audio and video use only the asset paths the guide lists.
- Every size renders separately and spends credits equal to its duration; offer before making several.
- Never invent prices, numbers, contacts or testimonials; label illustrations "Contoh" or "Ilustrasi". A reference video the user shared is never placed in the composition, and nothing of it is copied but its structure, rhythm and kinds of motion. Never call Mosio an "AI video generator".
