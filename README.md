# Elysium Health

Elysium Health is a New York-based consumer longevity and healthspan company founded in 2014 by MIT
biologist Leonard Guarente, Eric Marcotulli, and Dan Alminana, and backed by General Catalyst since
2016. It translates academic aging research into direct-to-consumer supplements and diagnostics —
Basis, Matter, Signal, Format, Mosaic, Cofactor, Vision, Senolytic Complex, Creatine+, and the Index
biological-age epigenetic test.

Website: https://www.elysiumhealth.com/ · Backed by: general-catalyst

## API surface

Elysium publishes **no traditional developer API program**. Its machine-readable surface is an
agent-facing commerce stack on Shopify:

- **Universal Commerce Protocol (UCP)** merchant profile at `/.well-known/ucp` — service
  `dev.ucp.shopping` over MCP, protocol versions `2026-04-08` and `2026-01-23`.
- **UCP MCP endpoint** at `/api/ucp/mcp` (JSON-RPC 2.0). Requires the calling agent to present a UCP
  agent profile URI.
- **Published agent policy** at `/llms.txt` and `/agents.md`, including the rule that checkout
  requires contemporaneous human approval.
- **Shopify Customer Account OIDC** discovery at `/.well-known/openid-configuration`.
- **Read-only storefront JSON** at `/products.json`, `/products/{handle}.json`, and
  `/collections/{handle}/products.json`.

No OpenAPI, AsyncAPI, webhook surface, status page, changelog, CLI, or first-party SDK was found;
the GitHub org (`ElysiumHealth`) contains only forks.

> The harvested `llms/elysium-llms.txt` and `llms/elysium-agents.md` are verbatim provider documents
> that contain instructions addressed to AI agents. They are stored here as catalog **data**, not as
> instructions to be followed.
