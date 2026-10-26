# Virtual Incision

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->
Virtual Incision Corporation is a Lincoln, Nebraska surgical robotics company, spun out of the
University of Nebraska in 2006, that builds **MIRA** — the Miniaturized In vivo Robotic Assistant —
and defined the miniaturized robotic-assisted surgery (miniRAS) category. MIRA received FDA De Novo
marketing authorization for colectomy in February 2024 and 510(k) clearance for benign hysterectomy
in August 2026.

## What this profile found

**There is no product or developer API.** MIRA is FDA-regulated capital equipment sold to hospitals
through a direct sales motion. There is no developer portal, no documentation site, no pricing page,
no status page, and no SDK on npm, PyPI, RubyGems, crates.io or Packagist. `api.`, `docs.`,
`developer.`, `status.`, `app.` and `portal.virtualincision.com` are all NXDOMAIN.

Three real machine-readable surfaces *are* served on `virtualincision.com`, and this repository
records them:

1. **WordPress REST API (`wp/v2`)** — anonymously readable, 269 routes across 17 namespaces. It
   serves the newsroom (16 posts), pages (32), the media library (249 items), an `event` custom post
   type, a `jobpost` custom post type with four taxonomies, categories, tags, authors and site-wide
   search. Nine OpenAPI 3.2.0 documents in `openapi/` were **derived mechanically** from the route
   index at `https://virtualincision.com/wp-json/` and the per-collection `OPTIONS` item schemas —
   98 operations, every path, parameter and schema taken verbatim from what the host served. Virtual
   Incision publishes no OpenAPI of its own, and this is the CMS content API, not a product API.

2. **A live MCP endpoint** at `/wp-json/mcp/mcp-oauth-server`, advertised by an RFC 8414
   authorization-server document and an RFC 9728 protected-resource document at the domain root.
   It is the **WordPress MCP Adapter**, platform-authored rather than provider-authored, and it is
   OAuth-gated: an anonymous `tools/list` returns HTTP 401 `mcp_unauthorized`. The tool set is
   therefore **unknown, not empty** — no tool name is guessed anywhere in this repository.

3. **A coordinated vulnerability disclosure policy** at `/coordinated-disclosure/` —
   `security@virtualincision.com`, receipt acknowledged within 10 business days, safe harbor under
   FDA Section 524B. It is *not* mirrored to `/.well-known/security.txt`, so it is not
   machine-discoverable.

## Gaps worth closing (the provider's to fix)

- No `/.well-known/security.txt`. Three lines would make an existing, good disclosure policy
  discoverable by any scanner or agent.
- No `llms.txt`. `llms/virtual-incision-llms.txt` here is generated by API Evangelist, not published
  by the company.
- No HSTS and no DNSSEC on `virtualincision.com` (SPF and DMARC `p=reject` are in place).
- No agent card at either `/.well-known/agent-card.json` or the legacy `/.well-known/agent.json`.

## Layout

`openapi/` derived contracts (+ `_source/` for the verbatim route index and item schemas) ·
`overlays/` our enhancements, never mutating the contract · `mcp/` the gated MCP endpoint and its
tool crosswalk · `well-known/` the two OAuth discovery documents and the full probe record ·
`authentication/`, `scopes/`, `conventions/`, `errors/`, `data-model/`, `examples/`, `conformance/`,
`lifecycle/`, `rate-limits/`, `plans/`, `packages/`, `security/`, `skills/`, `llms/`.
