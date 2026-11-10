---
name: storyboard
description: "Turn a brief into a content storyboard of 3-6 beats (role, on-screen text, key word, data, subject, weight) and show it for free with preview_storyboard (sketches of the content, then 1-3 fitting Mosio recipes as looks to pick from), so the user approves the content and picks a look before any HTML. Also for editing beats or switching the recipe while keeping the facts. Use for bikin storyboard, alur video, scene per scene, ganti recipe."
license: "Proprietary; adapted parts are MIT or Apache-2.0, see NOTICE.md"
metadata:
  brand: Mosio
---

# Content storyboard

This is one step of the `video` skill's flow (workspace, goal, brand, material (Notes, research, Discover, references), type, brief, look (the recipe), storyboard, build, review, preview, render, post). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

`brief.md` beside this file is the reference: read "Storyboard in two layers", "Show it and pick a recipe", "Layer 2, the recipe plan" and "Revisions".

1. **The brand, before writing a beat**, even when the user names the shop: `list_brands`, then `get_brand` with its `brand_id` (none, and the video is a story, a lesson or a creator's post: go on without one). Only what `get_brand` returns is the brand; the examples in `brief.md` (shop names, prices) are illustrations, never this brand's data.
2. **No brief yet?** Take the facts the user gave; ask only what is missing, in one batch of at most 5 questions (`brief.md`, "Questions"). With enough to go on, write the brief in a few lines and continue.
3. **Write layer 1** for the chosen look (`brief.md`, "Concept": the recipe is settled before the beats; a story, a lesson or a launch gets 2-3 concepts to pick from first), the content: 3-6 beats, each with a role (hook, problem, fact, step, compare, number, proof, offer, cta, payoff), the on-screen text in quotes (at most 7 words: a scrolling viewer reads one glance per beat), the key word, the data it uses, the subject and a weight (short, medium, long). No motion words and no seconds.
4. **Show it** with `preview_storyboard`: the beats and, in `recipes`, the chosen concept's recipe, or for a quick sale 1-3 recipes from `list_recipes` with the same kind of subject (only the user's recipe when they chose one). The card plays them in the brand's colours and offers every other recipe too. No look chosen yet: leave `recipes` out and pass `goal` and `ratio`, and the server recommends three. Ask "Isinya sudah pas? Mau tampilan yang mana?" and wait.
5. **Revise** the way "Revisions" says: a content edit changes only those beats and shows again.
   **A recipe change** (now, or after the content was approved): call `get_composition_guide` with the new `recipe` and the `brand_id` and compare its plan's kind of subject with the old one. Same kind: keep every beat, text and fact exactly, say in a few lines how the beats will be shown (technique per role, tempo), and do not ask for content approval again. Another kind (one product to an illustrated world, a conversation, characters): keep every fact and approved line, re-fit only the subjects and weights, show which beats changed, re-sketch with `preview_storyboard` in that recipe only, and ask once. A video already edited in Studio: say first that those edits do not carry over. Then offer to build the preview (the `composition` skill).
6. **Next**, after approval and a recipe pick: the `composition` skill (`get_composition_guide` with that `recipe` gives the plan, layer 2).

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
