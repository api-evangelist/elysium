# Elysium Health

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
