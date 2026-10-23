---
name: recipe
description: "Browse Mosio's recipes, the looks a video is made in (recipes the assistant composes from, and fill-in recipes where only the text changes), and make a video fast from a fill-in recipe: show_recipes for the gallery, list_recipes for ids and variables, preview_recipe, render after the user's OK. Use for pakai template, yang cepet aja, recipe apa aja, lihat contoh desain."
license: Proprietary
metadata:
  brand: Mosio
---

# Recipes

This is one step of the `video` skill's flow (brand, goal, type, brief, storyboard, recipe, build, review, preview, render). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

Every look is a **recipe**, in two kinds: recipes to compose from (you write the HTML from the recipe's plan and example; the `composition` skill) and **fill-in recipes** (fixed designs where only the text changes). Say "recipe" to the user; when the user says "template", they mean a fill-in recipe. `list_recipes` has the current ids; never hardcode them.

- **"What is there?"** `show_recipes` shows the gallery as a card (`category` narrows the fill-in ones). Do not list recipes in chat when a card can show them.
- **A fill-in recipe** (the user picked one, or wants it quick): follow `fill-in.md` beside this file. Read the brand first (`list_brands`, `get_brand`), take the variables and limits from `list_recipes` (`kind: "fill_in"`), fill them with the user's facts only, then `preview_recipe` (free, up to 3 alternatives). The user presses "Buat video" in the card; without a card, `render_recipe` only after they agree, then `get_status`.
- **A recipe to compose from**: write the content storyboard first (the `storyboard` skill), sketched in that recipe only, then the `composition` skill.
- Fill-in recipes exist for promo-type content (products, discounts, menus, services, reviews, openings, announcements). Tips, explainers, ads, app launches, chat stories and slideshows are composed.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
