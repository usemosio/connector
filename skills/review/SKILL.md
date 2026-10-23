---
name: review
description: "Check a Mosio video before the user sees it, or when they say something looks wrong: inspect_composition returns one still per beat and measured findings (text outside its box, covered text, off-canvas, contrast) with fix hints, then a checklist for whether the video is good, honest and works on mute. Use for cek videonya, teksnya kepotong, kok kurang bagus, review video."
license: "Proprietary; adapted parts are MIT or Apache-2.0, see NOTICE.md"
metadata:
  brand: Mosio
---

# Review a video

This is one step of the `video` skill's flow (brand, goal, type, brief, storyboard, recipe, build, review, preview, render). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

`review.md` beside this file is the checklist: read "How to look", "Never ship", "Truth", "Purpose on mute" and "Checklist every round".

1. **Get the HTML.** Your own composition: the HTML you are about to preview. A video that exists (`prj_…`): `get_project` for its current HTML (with the user's Studio edits) and its `version`.
2. **Look.** `inspect_composition` with that `html` (and `project_id` for an existing video) and `at` at the middle of each beat. It returns a contact sheet (one still per time plus the last frame, from the same engine as the render) and measured findings, each with a fix hint. It is free.
3. **Judge** the stills with the checklist, as a stranger watching once, on a phone, on mute. Findings you meant (an effect that overflows on purpose) get `data-layout-allow-overflow` / `data-layout-allow-overlap`.
4. **Fix and look again**, at most two rounds, then `preview_composition` so the user sees it. For an existing video, keep element ids and send `project_id` and `base_version`.
5. Tell the user in two or three lines what you found and changed. Never render: the user presses "Buat video" in the preview card, or agrees in chat.

A fill-in recipe has a fixed design: check only the text against its limits (`list_recipes`) and look at `preview_recipe`.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
