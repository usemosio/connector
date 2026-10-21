# Promo: sale, discount, strike-through price

A 6-15 s spot that announces **one offer** and gets people to act now: the discount lands, the old price is struck, the new price drops in, the code and deadline read, the CTA closes. Not a feature ad (see `ad.md`) and not a testimonial. Run `../brief.md` first; technical rules come from `get_composition_guide`.

## Pick the path

| Need                                                | Path                                                                                                        |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| One product with price and old price, standard look | template `promo-produk` (`judul`, `harga`, `harga_coret`, `badge`, `ajakan`)                                |
| Time-limited discount in percent                    | template `diskon-kilat` (`nama_usaha`, `persen`, `judul`, `produk`, `berlaku`, `ajakan`)                    |
| Custom look, promo code, counting price             | composition from style `flash-sale` (percent blast, ticker tape) or `product-hero` (one product, big price) |

Template path: `show_templates`, then `preview_template` with the full id (e.g. `promo-produk@1`), then `render_template`. Neither style ships a strike-through price or a code chip: add them from the recipes below.

## Ask before you start

Everything comes from the user; guess nothing: original and promo price (or original price and percent) in Rupiah; the promo code exactly as written; the deadline (date and time, or the true condition: "selama stok ada" only if that is the rule); what is discounted and where to buy (WhatsApp, Shopee, Tokopedia, the shop). If percent and prices disagree (Rp100.000 to Rp70.000 called "diskon 50%"), ask which is right before writing anything.

## Beats

| Beat              | Job                                                 | 6 s               | 10 s             | 15 s    |
| ----------------- | --------------------------------------------------- | ----------------- | ---------------- | ------- |
| Hook              | moment and shop name hit ("PROMO GAJIAN")           | 0-1               | 0-1.5            | 0-1.5   |
| Discount          | big number lands ("40%")                            | 1-2.5             | 1.5-4            | 1.5-4.5 |
| Strike price      | old price struck, new price drops                   | 2.5-4             | 4-7              | 4.5-8.5 |
| Product           | one product visual                                  | (cut)             | under price, 4-7 | 8.5-11  |
| Code and deadline | code chip and deadline, held still                  | (merged into CTA) | 7-8              | 11-12.5 |
| CTA               | "Belanja sekarang", shop, contact, held 2 s or more | 4-6               | 8-10             | 12.5-15 |

- One beat, one job. The hook never grows; longer cuts give each beat more breath. The discount number is the biggest thing on screen (Archivo Black or Bebas Neue).
- Frame by price: expensive items use percent ("40%" beats "Hemat Rp52.000"), cheap items use the amount ("Hemat Rp5.000" beats "22%").

**Two hard rules.** (1) The number is the message, so it must be right: compute percent from one data object and never type a price twice; a wrong price is worse than no video. (2) One offer per video: one discount, one code, one CTA. Two promos means two videos.

## Strike-through recipe

Strike first, then drop the new price. Both at once looks like a glitch.

```html
<div id="persen" class="persen">31%</div>
<span class="lama"><span id="awal">Rp129.000</span><i id="coret"></i></span>
<div class="topeng"><div id="promo">Rp129.000</div></div>
```

```css
.lama {
  position: relative;
  opacity: 0.55;
}
#coret {
  position: absolute;
  left: -4%;
  right: -4%;
  top: 52%;
  height: 8px;
  background: var(--aksen);
  transform-origin: left center;
}
.topeng {
  overflow: hidden;
}
#promo {
  font-variant-numeric: tabular-nums;
}
```

```js
const HARGA = { awal: 129000, promo: 89000 }; // from the user, the only place prices are typed
const rp = (n) => "Rp" + n.toLocaleString("id-ID");
document.getElementById("awal").textContent = rp(HARGA.awal);
document.getElementById("persen").textContent =
  Math.round((1 - HARGA.promo / HARGA.awal) * 100) + "%";
const el = document.getElementById("promo"),
  o = { v: HARGA.awal };
tl.fromTo("#coret", { scaleX: 0 }, { scaleX: 1, duration: 0.4, ease: "power2.out" }, 4.0)
  .fromTo("#promo", { yPercent: 110 }, { yPercent: 0, duration: 0.5, ease: "expo.out" }, 4.3)
  .to(
    o,
    {
      v: HARGA.promo,
      duration: 0.9,
      ease: "power3.out",
      onUpdate: () => {
        // Round to thousands while counting so digits do not flicker; exact value at the end.
        el.textContent = rp(o.v === HARGA.promo ? o.v : Math.round(o.v / 1000) * 1000);
      },
    },
    4.3,
  )
  .to("#promo", { scale: 1.1, duration: 0.15, yoyo: true, repeat: 1, ease: "power2.out" }, 5.2);
```

- `tl` is the guide's paused main timeline; the JS overwrites the placeholder text. The strike uses `scaleX`, not `width`; `transform-origin` in CSS is fine, an initial `transform` in CSS is not.
- The new price holds still at least 1.2 s after it lands. Rupiah format: `Rp89.000` (dot thousands, no space, no ",00").

## Promo code

- A chip: dashed accent border, slightly lighter fill, bold wide-tracked type (Space Grotesk 700 or a Google Fonts mono). Held still at least 2 s: a moving code cannot be read or screenshotted.
- Write it exactly as given. Urgent codes read stronger (illustrative: "GAJIAN25", "KILAT40"), but never replace the real one.

## Deadline: text, never a clock

People watch any time, even after the promo ends, so the deadline is text that stays true: "Berakhir Minggu, 23.59 WIB", "Hanya 12.12", "Sampai 31 Desember", "Hari ini saja" (only if the video is for that day).

- The `flash-sale` style has a static deadline block: "Berakhir" plus a chip such as "Minggu, 23.59 WIB", stamped in and then held. Replace the chip with the user's deadline, or remove the whole block if there is none. Never add a countdown clock (no ticking hours, minutes, seconds). For countdown energy, use a "3, 2, 1, MULAI" count-in in the hook.
- Urgency must be real ("Stok tinggal 5" only if the user said so). Hold the deadline 2 s or more; never blink it.

## Motion: energetic, not chaotic

- One entrance ease for all beats (`back.out(1.6)` or `expo.out`) so the speed feels sure.
- One saving accent (bright red, yellow or green) on percent, strike line and button.
- Change beats with hard cuts or fast wipes. No floaty drift or parallax; the background lives quietly (grid drift, tape with a fixed `repeat`).
- Product: small `data:` image only (200,000 char limit), or the product name big with a simple SVG shape. No photos from the internet.

## One layout, many products

Build one composition; per product change only the data: numbers in `HARGA`, marked text in the HTML (`<!-- GANTI per produk -->` on name, code, deadline). Layout, colours and timing stay.

- Check `HARGA.promo < HARGA.awal`, code not empty if used, deadline present; test the longest name and widest price (Rp1.250.000) inside the safe zone.
- Preview one product until it is right, then the rest. Each render costs credits equal to its length: state the total before making them all. Title each video with its product ("Promo Gajian: Sneakers Langkah Run").

## Before you send

- Prices exact in Rupiah from one source; strike first, price second. One offer, one code, one CTA; the discount is the biggest element.
- The deadline is text that is true whenever watched, held 2 s or more. Prices, code, deadline and contact come from the user or brand kit.

More in `skill_library`: digit swaps `short-form/countdown-video/references/digit-flip.md`, count-up `hyperframes/hyperframes-animation/blueprints/dataviz-countup.md`, ticker tape `hyperframes/hyperframes-animation/blueprints/ticker-takeover.md`, CTA button `hyperframes/hyperframes-animation/blueprints/cta-morph-press.md`. Counting rules: `../../video-composition/craft/numbers.md`.

## Menus, services, openings and announcements

The same one-message rule, without the price drop:

| Kind                                        | Template                                          | Style to start from | The one thing that must read                   |
| ------------------------------------------- | ------------------------------------------------- | ------------------- | ---------------------------------------------- |
| Menu                                        | `menu-fnb` (2-4 items), `menu-spesial` (one item) | `menu-board`        | each item and its price, from the user         |
| Service                                     | `jasa-layanan`                                    | `product-hero`      | the service, a "mulai dari" price, the contact |
| Store or branch opening                     | `buka-baru`                                       | `kinetic-type`      | the date and a short address                   |
| Announcement (closed, new hours, new stock) | `pengumuman`                                      | `kinetic-type`      | what changes and from when                     |

- Dates, hours and addresses come only from the user. Write them the way customers read them ("Sabtu, 1 November", "Tutup 24-26 Desember").
- The fact holds still at least 2 s; the ending still carries the name and contact.

## Adapted from

- [iart-ai/ecommerce-video-skills](https://github.com/iart-ai/ecommerce-video-skills) (MIT, © iart.ai), commit `2bd0dd1`: `skills/promo-video/SKILL.md`, `skills/promo-video/references/promo-components.md`.

Changes by Mosio: translated, condensed and adapted to Mosio's composition rules (one HTML file, paused GSAP timeline, no audio, no CDN); Remotion code replaced by a GSAP recipe and prices by Rupiah. The real-time countdown, batch scripts and file validation were dropped and replaced by a text deadline, a per-product data block and previews; added the template path and Indonesian campaign examples. Full licence texts: `skills/licenses/` in the Mosio repo, or NOTICE.md beside this skill in the plugin.
