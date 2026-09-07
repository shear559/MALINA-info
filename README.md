# MALINA

**A Hebrew RTL gift storefront for browsing gift boxes and accessories, personalizing an order and managing the shop behind it.**

[Visit the storefront preview](https://malina-pi.vercel.app)

<p align="center">
  <img src="assets/preview.webp" alt="MALINA storefront preview" width="100%">
</p>

## Shopping and operations

The storefront includes category and price filters, product details, a cart, wishlist, coupons and gift options. Gift messages, wrapping and recipient details travel with the order flow.

The implementation now includes customer accounts and an authenticated management console for catalogue quality, stock, orders, returns, fulfilment, promotions and store settings. A Vercel API proxy connects the framework-free frontend to a Cloudflare Worker, D1 and R2.

## Current stage

This is a storefront preview. Customer authentication, manager access and order submission require the remaining Turnstile and Worker configuration before launch. Catalogue, supplier and merchant details also need approval. Checkout does not process card payments or claim that an order has been paid.

The browser keeps a local cart and wishlist; the commerce backend provides shared records and server-side order pricing once the required services are configured.

## Screenshots

<p align="center">
  <img src="assets/shop-grid.webp" alt="MALINA catalogue interface preview" width="100%">
</p>
<p align="center">
  <img src="assets/product-page.webp" alt="MALINA product interface preview" width="100%">
</p>
<p align="center">
  <img src="assets/mobile-home.webp" alt="MALINA homepage on mobile" width="45%">
</p>

## Stack

`HTML` · `CSS` · `JavaScript` · `Vercel Functions` · `Cloudflare Workers` · `D1` · `R2` · `Turnstile`

Source is private. Built by [@shear559](https://github.com/shear559).
