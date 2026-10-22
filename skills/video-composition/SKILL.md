---
name: video-composition
description: "Write a video as an HTML composition that Mosio renders: one file, one paused GSAP timeline, the brand's colours and fonts, a recipe's plan and example. Use for any custom motion video after the brief and storyboard are approved, or when the user shares HTML to turn into a video. The rules come from the Mosio server."
license: Proprietary
metadata:
  brand: Mosio
---

# Mosio video composition

You design the video yourself as one HTML file (a Hyperframes composition): layout, typography, colour and motion. Mosio checks it, shows a moving preview in the card, and renders it to MP4 on its servers. The looks you build from are **recipes** (10 today; `list_recipes` has the current list). Say "recipe" to the user.

Everything below is read from the Mosio server when you need it, so it is always current. Do not work from memory of an earlier version.

## Flow

1. Follow the `video` skill first: brand (`list_brands`, `get_brand`), goal, type, brief and content storyboard, `preview_storyboard`, the user's approval and recipe pick.
2. Call `get_composition_guide` with the `brand_id` and the chosen `recipe`. It returns the technical rules (root element, sizes and safe zones, the one paused GSAP timeline, what is not allowed, assets and fonts), the brand brief, the recipe's plan (a technique per beat role and seconds per weight) and its example code. The rules in it win over anything you remember.
3. For craft beyond the guide, read `skill_library`: `mosio/video-composition/craft/motion.md`, `transitions.md`, `kinetic-type.md`, `captions.md`, `camera.md`, `numbers.md`, `logo.md`, `sizes.md`. For a ready-made effect, `search_registry` then `get_registry_item`.
4. Write the full HTML from the approved beats and the recipe's plan. The recipe gives the structure; the brand wins on colours, fonts, logo, words and motion feel. Change the example's text, colours, scenes and rhythm; never send it as is.
5. Look before the user does: `inspect_composition` with the same `html` and `at` at the middle of each beat returns one contact sheet and measured findings (text outside its box, covered text, off-canvas, contrast) with fix hints. Fix, or mark an effect you meant with `data-layout-allow-overflow` / `data-layout-allow-overlap`, and inspect again, at most twice. Judge the stills with `mosio/video/review.md`.
6. `preview_composition` (free) with the inspected HTML. If it is rejected, read `details.problems` (lint code, message, fix), fix, and send again.
7. The user watches the preview in the card and presses "Buat video" themselves (credits equal to the duration), or "Buka di Studio". In apps without cards, call `render_composition` only after the user agrees, then `get_status`, and share the download link and `project_url`.
8. A change to an existing video is a new version of the same video: `get_project` (current HTML, with the user's Studio edits, and its `version`), edit that HTML, keep element ids, then `preview_composition` and `render_composition` with `project_id` and `base_version`.

## Always

- One HTML file, one paused GSAP timeline registered as `window.__timelines["main"]`, no clock, randomness, CSS animations, audio, video, iframes or network; only the assets the guide lists.
- Every size renders separately and spends credits equal to its duration; offer before making several.
- Never invent prices, numbers, contacts or testimonials; label illustrations "Contoh" or "Ilustrasi". Never call Mosio an "AI video generator".
