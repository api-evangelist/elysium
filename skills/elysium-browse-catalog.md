---
name: Browse the Elysium Health catalog
description: Read Elysium Health product and collection data with no authentication, using the read-only storefront JSON endpoints the store publishes for agents.
api: https://www.elysiumhealth.com/products.json
generated: '2026-07-20'
method: generated
source: https://www.elysiumhealth.com/llms.txt
operations:
  - GET /products.json
  - GET /products/{handle}.json
  - GET /collections/{handle}/products.json
  - GET /search?q={query}&type=product
---

# Browse the Elysium Health catalog

Elysium Health sells longevity supplements and the Index biological-age test from a Shopify
storefront at `https://www.elysiumhealth.com`. The store's published agent policy (`/llms.txt`,
mirrored from `/agents.md`) explicitly permits unauthenticated read-only browsing of catalog data.
Use this skill when the user wants product facts, prices, or availability — not when they want to buy
(see `elysium-agentic-purchase.md` for that).

## Rules

- **No credentials are needed and none should be sent.** These endpoints are public.
- **Respect `robots.txt`.** `/search`, `/cart`, `/checkout`, `/account`, `/orders`, and `/policies/`
  are disallowed to crawlers. Only fetch `/search` for a live, user-initiated lookup — never to
  enumerate or mirror the catalog.
- **Do not mirror the store.** The catalog is small (11 products as of 2026-07-20); fetch what the
  user actually asked about.
- Back off on `429`.

## Steps

1. **List everything.** `GET https://www.elysiumhealth.com/products.json` returns a `products[]`
   array. Each product carries `id`, `title`, `handle`, `body_html`, `vendor`, `product_type`,
   `tags`, `options[]`, `variants[]`, and `images[]`.
2. **Read one product.** `GET https://www.elysiumhealth.com/products/{handle}.json` — handles in use
   include `basis`, `matter`, `signal`, `format`, `mosaic`, `cofactor`, `vision`,
   `senolytic-complex`, `creatine`, `index`, and `longevity-starter-pack`.
3. **Pull prices and stock.** Price lives on the variant, not the product: read
   `variants[].price`, `variants[].compare_at_price`, `variants[].sku`, and
   `variants[].available`. A product with several variants (sizes, subscription vs one-time) will
   differ in price per variant — always name which variant a quoted price belongs to.
4. **Filter by concern.** `product_type` maps a product to an aging domain — `Cellular Aging`,
   `Cognitive Aging`, `Immune Aging`, `Metabolic Aging`, `Skin Aging`, `Eye Longevity`,
   `Muscle Aging`, `Rate of Aging`. Use it to answer "what do you have for X?".
5. **Scope to a collection.** `GET /collections/{handle}/products.json`, or
   `GET /collections/all` for the human-readable page.
6. **Live lookup only.** `GET /search?q={query}&type=product` for a user-initiated search.

## Reporting back

Elysium sells dietary supplements and a consumer epigenetic test. When relaying product claims,
attribute them to Elysium's own marketing copy (`body_html`) rather than restating them as
established medical fact, and do not turn a catalog answer into personalized health advice. The
company's science pages (`/pages/science`, `/blogs/clinical-trials`) are the place to point a user
who wants the underlying evidence.

## Related artifacts

- `data-model/elysium-data-model.yml` — full entity/field graph for these payloads
- `conventions/elysium-conventions.yml` — cross-cutting rules
- `errors/elysium-problem-types.yml` — error envelope
