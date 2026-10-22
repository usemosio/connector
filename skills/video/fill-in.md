# Fill-in recipes

A fill-in recipe is a fixed Mosio design where only the text changes: fast, with a tested layout. Use it when one fits the request and the user only needs their own text and colours, or picks one from the gallery, or wants it quick. For anything else, write a composition from a recipe (`get_composition_guide`); a fill-in recipe can still serve as a reference there.

The user chooses through the cards, not a text list. Do not write a list of recipes in the chat or ask the user to pick one in the chat; in apps that support it, the interactive card shows the options. Only in apps without cards (a terminal, for example) summarise the options as text.

1. Call `list_recipes` to read each fill-in recipe's variables and length limits (`kind: "fill_in"`). This is data for you; do not summarise it to the user.
2. If the user does not know what they want, or asks which recipes exist, call `show_recipes` (a moving gallery of every recipe, optionally filtered by `category`). The user's pick arrives as the next message: a compose recipe means you write a composition with it, a fill-in recipe means the steps below.
3. When the content is clear, pick the best-fitting fill-in recipe:
   - a product with a price or a struck-through price: `promo-produk`
   - a time-limited discount: `diskon-kilat`
   - several menu items with prices: `menu-fnb`; one signature item: `menu-spesial`
   - a service (laundry, repairs, salon, tutoring): `jasa-layanan`
   - a customer review: `testimoni`; a shop or branch opening: `buka-baru`; closures, opening hours or other news: `pengumuman`

   Then call `preview_recipe` with the full id (e.g. `promo-produk@1`) and its variables, plus 2-3 other fitting fill-in recipes in `alternatives` with the text adapted to their variables. This is free: it renders nothing and spends no credits, but validates the variables with the same rules. The user watches the moving preview, picks a design and colours, edits the text, and presses "Buat video" themselves; the card tells you their choice and the video id.
4. Always preview first, even when the user wants it fast: `preview_recipe` is free and quick, `render_recipe` spends credits. Call `render_recipe` only after the user has seen the preview (or, in apps without cards, your summary of the recipe and its text) and asks in the chat to render it ("render", "buat videonya", "oke, jadikan"). "Yang cepet aja" or "langsung buat" means pick the recipe and preview it now without questions, not render unseen. Use the same variables, a unique `idempotency_key` for that request, and the default size 1080×1920 unless the user chose another.
5. `render_recipe` returns an id straight away. If no card is shown, call `get_status` every 5-10 seconds until the status is `done` (usually under a minute), then share the download link and the video's `project_url` (where the user can watch it and tweak it in Studio). If it is `failed`, explain the message in plain words.

## Writing the text
- The variables are on-screen text: write them in the user's language (Indonesian by default for Indonesian businesses), friendly and short, the way a shop owner talks to customers. No piles of exclamation marks, no claims that cannot be proven ("termurah se-Indonesia").
- Respect each variable's length limit; text that is too long is rejected. Shorter reads better on a phone.
- Prices in Rupiah format: `Rp25.000`. A struck-through price only when there really is an old price.
- Never invent prices, addresses, contact numbers or opening hours. Take them from `get_brand` (products and prices, contact), ask first, or leave optional variables empty.
- The business name, colours and contact come from the chosen brand (`brand_id`) when not sent. Send them only when the user wants something different. Write the copy in the brand's voice.
