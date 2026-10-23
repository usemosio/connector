---
name: storyboard
description: "Turn a brief into a content storyboard of 3-6 beats (role, on-screen text, key word, data, subject, weight) and sketch it for free in 1-3 Mosio recipes with preview_storyboard, so the user approves the content and picks a look before any HTML. Also for editing beats or switching the recipe while keeping the content. Use for bikin storyboard, alur video, scene per scene, ganti recipe."
license: "Proprietary; adapted parts are MIT or Apache-2.0, see NOTICE.md"
metadata:
  brand: Mosio
---

# Content storyboard

This is one step of the `video` skill's flow (brand, goal, type, brief, storyboard, recipe, build, review, preview, render). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

`brief.md` beside this file is the reference: read "Storyboard in two layers", "Show it and pick a recipe", "Layer 2, the recipe plan" and "Revisions".

1. **Brand, always first**, even when the user names the shop: `list_brands`, then `get_brand` with its `brand_id`. Only what `get_brand` returns is the brand; the examples in `brief.md` (shop names, prices) are illustrations, never this brand's data.
2. **No brief yet?** Take the facts the user gave; ask only what is missing, in one batch of at most 5 questions (`brief.md`, "Questions"). With enough to go on, write the brief in a few lines and continue.
3. **Write layer 1**, the content: 3-6 beats, each with a role (hook, problem, fact, step, compare, number, proof, offer, cta, payoff), the on-screen text in quotes (at most 7 words), the key word, the data it uses, the subject and a weight (short, medium, long). No motion words and no seconds.
4. **Show it** with `preview_storyboard`: the beats and 1-3 fitting recipes from `list_recipes` (only the user's recipe when they chose one). Ask "Isinya sudah pas? Mau recipe yang mana?" and wait.
5. **Revise** the way "Revisions" says: a content edit changes only those beats and shows again.
   **A recipe change** (now, or after the content was approved) keeps every beat, text and fact exactly and does not ask for content approval again: call `get_composition_guide` with the new `recipe` and the `brand_id`, say in a few lines how the beats will be shown with that recipe's plan (technique per role, tempo), optionally re-sketch with `preview_storyboard` in that recipe only, and offer to build the preview (the `composition` skill).
6. **Next**, after approval and a recipe pick: a recipe to compose from goes to the `composition` skill (`get_composition_guide` with that `recipe` gives the plan, layer 2); a fill-in recipe goes to the `recipe` skill.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
