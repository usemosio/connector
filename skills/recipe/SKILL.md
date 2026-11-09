---
name: recipe
description: "Pick the Mosio recipe a video is composed from: a look with a plan per beat and example code (or, for a library recipe, a style's art direction). show_recipes shows the gallery in the brand's colours, list_recipes gives the ids with what each is for; the chosen recipe then shapes the storyboard and the composition. Use for pakai template, recipe apa aja, lihat contoh desain, gaya videonya apa aja, yang cepet aja."
license: Proprietary
metadata:
  brand: Mosio
---

# Recipes

This is one step of the `video` skill's flow (goal, brand, type, brief, concept (the recipe), storyboard, build, review, preview, render). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

Every look is a **recipe**: you write the HTML from the recipe's plan and example (the `composition` skill); a library recipe (`library: true`, from an open style library) gives a style's art direction instead of a plan and example code. Say "recipe" to the user; when the user says "template", they mean a recipe. `list_recipes` has every current id with what it is for: pick from it, not from memory, because recipes are added and retired between plugin versions.

- **"What is there?"** `show_recipes` shows the gallery as a card. Do not list recipes in chat when a card can show them.
- **Which one:** by the content, the size and the feel. `list_recipes` says what each recipe is for (description, tags, size), and the type file in the `video` skill (`types/<type>.md`) names the usual starting point for its type.
- **The user picked one** (in the gallery, or a prompt from the dashboard's Recipes names it): write the content storyboard first (the `storyboard` skill), sketched in that recipe only, then the `composition` skill.
- **"Pakai template aja" or "yang cepet"**: the best-fitting recipe, a short brief and the beats as decisions, then straight to a free preview (the `composition` skill). Never guess facts; the render still waits for the user.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
