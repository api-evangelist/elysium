---
name: Purchase from Elysium Health over UCP with buyer approval
description: Use Elysium Health's Universal Commerce Protocol MCP endpoint to search, cart, and check out — stopping for explicit human approval before any payment is completed.
api: https://www.elysiumhealth.com/api/ucp/mcp
generated: '2026-07-20'
method: generated
source: https://www.elysiumhealth.com/llms.txt
operations:
  - search_catalog
  - create_cart
  - create_checkout
  - update_checkout
  - complete_checkout
---

# Purchase from Elysium Health over UCP

Elysium Health's Shopify storefront implements the **Universal Commerce Protocol** (UCP) for
agent-driven commerce. The tool names and the step order below are the ones Elysium publishes
verbatim in `/llms.txt`; the capability set is the one advertised at `/.well-known/ucp`.

## The hard rule: checkout requires a human

Both of the store's published policies say the same thing, and this skill does not proceed without
it:

- `/llms.txt`: *"Checkout requires human approval. Agents must not complete payment without
  explicit buyer consent."*
- `/robots.txt`: *"Checkouts are for humans. Automated scraping, buy-for-me agents, or any
  end-to-end flow that completes payment without a final human review step is not permitted."*

So: **stop before `complete_checkout`.** Present the cart, the total, the shipping method, and the
address to the user, and call `complete_checkout` only after they approve that specific purchase in
that moment. If you cannot get contemporaneous approval, do not complete the purchase — hand the
user the checkout URL and let them finish it themselves. Do not persist consent from an earlier
turn, and do not treat a general "yes, buy me supplements" as approval of a specific total.

## Before you start

1. **Discover.** `GET https://www.elysiumhealth.com/.well-known/ucp` and confirm the version you
   intend to speak is in `supported_versions`. Current: `2026-04-08` (latest stable); `2026-01-23`
   is also served.
2. **Know the endpoint.** `POST https://www.elysiumhealth.com/api/ucp/mcp` with
   `Content-Type: application/json`, JSON-RPC 2.0. `tools/list` enumerates the live tool schemas.
3. **Present an agent profile URI.** Calling `tools/list` without one returns HTTP 422 /
   JSON-RPC `-32001` `invalid_profile_url` ("Missing profile uri"). The server will not transact
   with an agent it cannot identify.
4. **Pass buyer context.** Send `context.address_country` and `context.currency` so prices and
   availability are correct.

## Steps

1. `search_catalog` — find products matching the buyer's stated intent.
2. `create_cart` — add the chosen variants.
3. `create_checkout` — open the purchase flow.
4. `update_checkout` — set the shipping address and method. Elysium's fulfillment capability
   declares `allows_multi_destination.shipping: false` and only the `["shipping"]` method
   combination, so one destination per checkout.
5. **Pause. Show the buyer the full order and get explicit approval.**
6. `complete_checkout` — only after step 5.

Registered payment handlers are `dev.shopify.shop_pay`, `dev.shopify.card`, and `com.google.pay`.
Card data is handled by Shopify and Google Pay, never by the agent.

## Failure handling

- `429` — rate-limited per IP; back off and retry.
- `-32001 invalid_profile_url` — you did not present an agent profile URI.
- Any error arrives as a JSON-RPC 2.0 `error` object with `data.code`, `data.content`, and often a
  `data.continue_url` a human can open. When you cannot recover, give the user the
  `continue_url` rather than retrying blind.

## A note on what is being bought

These are dietary supplements and a consumer epigenetic test, not prescribed treatment. Do not
recommend a purchase on health grounds, and do not infer a regimen for the user — buy what they
asked for.

## Related artifacts

- `mcp/elysium-mcp.yml` — server manifest and live probe result
- `conventions/elysium-conventions.yml` — flow, rate limits, human-in-the-loop rule
- `errors/elysium-problem-types.yml` — error envelope
- `authentication/elysium-authentication.yml` — Customer Account OIDC, if the buyer signs in
