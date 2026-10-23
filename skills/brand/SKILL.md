---
name: brand
description: "Use the workspace's Mosio brand in a video: list_brands, ask which one when there are several, get_brand for voice, colours, fonts, logo, contact, products and prices, and pass the same brand_id to every video tool. Also sets up a new brand (create_brand) or changes one (update_brand) when the user asks. Use for pakai brand aku, warna tokoku, ganti harga di brand, tambah produk, bikin brand baru, brand yang mana."
license: Proprietary
metadata:
  brand: Mosio
---

# Brand

This is one step of the `video` skill's flow (brand, goal, type, brief, storyboard, recipe, build, review, preview, render). Do this step, then offer the next one; never render before the user has seen a preview and agreed.

1. `list_brands`. One brand: use it. Several: ask which one, unless the request already names it (a prompt from the dashboard carries `brand_id`). A video tool that answers `brand_required` lists the brands in `details.brands`: ask the user, never pick one yourself.
2. `get_brand` with that `brand_id`: business, voice (tone, how it addresses customers, words to avoid), colours, heading and body fonts, motion style, contact, products with prices, and the logo. What it sets is answered: never ask it again. What it lists as not set: ask when the video needs it, or choose freely when it does not (a colour, a font).
3. Pass the same `brand_id` to every tool that makes the video: `preview_storyboard`, `get_composition_guide`, `preview_composition`, `inspect_composition`, `preview_recipe`, and the renders. A new version of an existing video keeps that video's brand.
4. In a composition the brand wins over the recipe on colours, fonts, logo, words and motion feel (`get_composition_guide`, "Recipe and brand"). The logo is at the path the guide names (`assets/brand/logo.<ext>`).
5. **For this video only**, the user's words win without touching the brand: a promo price or a new number in the conversation is used and noted in one line.
6. **Changing the brand itself**, only when the user asks for it ("ganti harga di brand", "tambah produk", "warna brand jadi merah"): read `get_brand`, then `update_brand` with the `brand_id` and only the fields that change (`produk` replaces the whole list: send the full list with the change). Say in one line what changed. Unsure whether they mean this video or the brand: ask.
7. **A new brand** (no brand yet, or another business or product line): `create_brand` with the facts the user gave; never invent prices or contacts. A workspace holds as many brands as its plan allows (`brand_limit`): say so plainly. The logo is uploaded in the Mosio dashboard, on the Brand page.

- Say a fact comes from the brand only after `get_brand` returned it: `list_brands` gives names and taglines, nothing else. Never claim "best seller", an audience, a price or a contact the brand or the user did not give.
- Never invent prices, discounts, numbers, contacts, addresses, hours, ratings or testimonials: ask, or leave them out. What the user says in this conversation wins over the brand; note the difference in one line.
- Never call Mosio an "AI video generator": the motion is written as code.
