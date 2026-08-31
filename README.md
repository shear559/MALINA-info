# MALINA

**A Hebrew, right-to-left gift boutique for the Israeli market: curated gift boxes and accessories, built as a static site with no framework, no build step and no backend.**

Live: **https://malina-pi.vercel.app**

<p align="center">
  <img src="assets/preview.webp" alt="MALINA — the live site" width="100%">
</p>

## Why the storefront is hand-written static HTML

27 flat HTML pages, one 168 KB stylesheet, a ~1,500-line `shop.js`, and a `data.js` catalogue of 31 entries across 8 categories, with the coupon rules and store settings kept as data beside them. No framework, no bundler, no storefront platform: Vercel serves the files as committed. Cart, wishlist, coupon and gift state live in `localStorage`; checkout has no payment gateway and builds a `wa.me` deep link carrying only the order details, so card data never touches the site. That link still points at the `972500000000` placeholder: the merchant's real number has to be set before it routes a live order. Nothing invalidates caches for you, so every asset reference carries a manual cache-buster (`?v=41` today) bumped across every file in one commit.

## Deleting fabricated social proof from markup and structured data

The store used to present numbers nobody had measured: review and customer counts, an "ordered today" counter, a press strip, a return rate, and a catalogue size contradicting its own product data. The worse half was invisible. `product.html` synthesised each product's reviews deterministically from its product id and emitted them as `Review` nodes plus an `AggregateRating` inside the Product JSON-LD: invented ratings handed to search engines as fact.

It came out as a *feature removal*, not a copy edit: the star chip, the rating filters, the "high rating" sort branch, the review section and form, the live-viewer counter, and the `rating` / `reviews` fields on every product. No product in `assets/data.js` carries a `rating` or `reviews` key (a sanitizer strips both if they ever reappear), and product JSON-LD emits `Product`, `Offer`, `MerchantReturnPolicy` and `OfferShippingDetails`, with no `AggregateRating` and no `Review` node.

## Threading gift state from product page to confirmation

Gift state must survive four surfaces with no server behind it: the handwritten message (200 characters), wrap choice, ship-to-recipient address and hide-price flag ride in `localStorage` beside the cart, echoed in the summary and confirmation.

The same statelessness produced a real bug. The coupon was first persisted as a fixed shekel amount computed at apply time, so it froze while the cart kept changing; it is now stored as a rule, `{code, type, value}`, recomputed against the live subtotal on both cart and checkout. The original storage-key prefix was left alone despite carrying an earlier project's name: renaming keys on deploy would silently empty the cart of every customer who already had one.

## Hardening the edge and cutting third-party image dependencies

`vercel.json` ships a CSP whose `img-src` is `'self' data:`: no third-party image dependency, every photo regenerated on one shared art-direction prompt and stored locally as WebP, replacing a stock-photo CDN. `X-Robots-Tag` is `noarchive` site-wide, reopened for SEO after an earlier blanket block, while cart, checkout, login, account and wishlist keep `noindex, nofollow, noarchive, nosnippet`.

## How it was verified

The anti-clone layer blanks automated browsers, so verification is layered:

- a jsdom integration harness loading the *real* cart and checkout pages, 18/18 on the flow pass;
- headless-Chrome screenshots on every surface, desktop and mobile, plus functional smoke tests on the product page and the checkout happy path;
- a `curl` pass over the deployed markup, 12/12 expected markers;
- 9 pages clean in Chromium — zero console errors, no overflow, valid JSON-LD — with the same checks re-run against the deployed site rather than the local build, across home, shop, PDP, about and `vs-kipi`.

A live store with no test harness does not get a ground-up rewrite: two such requests were answered with shortest-diff passes instead.

## Stack

`HTML` `CSS` `JavaScript`, no framework or build step. `GSAP 3.13` and `Motion 10.18` lazy-loaded from CDN, gated behind `prefers-reduced-motion` and a `@media (scripting: none)` fallback. JSON-LD structured data, a 52-URL sitemap with 24 `<image:image>` entries, and Vercel static hosting with security headers, a custom 404 and a site-wide accessibility widget.

## Screenshots

<p align="center">
  <img src="assets/shop-grid.webp" alt="Shop page — all 31 products with category and price filters" width="100%">
</p>
<p align="center">
  <img src="assets/product-page.webp" alt="Product page — what's in the box, delivery estimate and sticky add-to-cart" width="100%">
</p>
<p align="center">
  <img src="assets/mobile-home.webp" alt="Home page on a 390px phone" width="45%">
</p>

Source is private. Built by [@shear559](https://github.com/shear559).
