---
name: virtual-incision-newsroom-monitor
description: Track Virtual Incision regulatory and clinical announcements (FDA clearances, first-procedure milestones, leadership changes) from the machine-readable newsroom, without scraping HTML.
api: virtual-incision:virtual-incision-posts-api
generated: '2026-09-04'
method: generated
source: openapi/virtual-incision-posts-api-openapi.yml, openapi/virtual-incision-taxonomy-api-openapi.yml, openapi/virtual-incision-search-api-openapi.yml
operations:
  - getPosts
  - getPostsById
  - getCategories
  - getSearch
---

# Monitor the Virtual Incision newsroom

Virtual Incision publishes no developer API. Its press releases are, however, served as JSON by the
WordPress REST API behind `virtualincision.com`, anonymously and without a key. Use that instead of
parsing the news page.

**Base URL:** `https://virtualincision.com/wp-json`
**Auth:** none for reads. Do not send credentials.

## 1. Find the categories worth watching

`getCategories` — `GET /wp/v2/categories?_fields=id,name,slug,count`

At the last profile the site carried three: `press-release` (15 posts), `news` / "Recent News" (7),
and `uncategorized` (0). Resolve the id at runtime; do not hard-code it.

## 2. Poll for new posts since your last check

`getPosts` — `GET /wp/v2/posts?after=<ISO8601>&orderby=date&order=desc&per_page=20&_fields=id,date,slug,link,title,excerpt,categories`

- `after` and `modified_after` both take an ISO 8601 date-time and are the correct incremental cursor.
- Read `X-WP-Total` and `X-WP-TotalPages` from the response headers for the result count, and follow
  `Link: rel="next"` rather than incrementing `page` blindly.
- Keep `per_page` at or below **100**; above it the request fails with `rest_invalid_param` (HTTP 400).
- `_fields` trims the payload. A full post object is large; ask only for what you use.

## 3. Read one release in full

`getPostsById` — `GET /wp/v2/posts/{id}?_fields=id,date,link,title,content`

`content.rendered` is HTML, not markdown, and `title.rendered` carries HTML entities
(`&nbsp;`, `&#8482;`). Decode before you compare strings.

## 4. Search across the whole site

`getSearch` — `GET /wp/v2/search?search=MIRA&per_page=10`

Returns polymorphic hits (`id`, `title`, `url`, `type`, `subtype`) spanning posts, pages and the
custom post types, which is the fastest way to find a topic that is not in the newsroom.

## Cadence and courtesy

- Responses carry `Cache-Control: max-age=600` and are usually served by Cloudflare
  (`cf-cache-status: HIT`), so polling faster than every 10 minutes buys nothing.
- `robots.txt` requests `Crawl-delay: 10`. Honour it. No rate limit is published and **no**
  `X-RateLimit-*`, `RateLimit-*` or `Retry-After` header is returned, so you get no warning before an
  undocumented edge protection engages — back off on any 429 or 5xx and retry with jitter.

## Errors

Errors use the WordPress envelope, not RFC 9457:
`{"code":"rest_no_route","message":"...","data":{"status":404}}`. Branch on `code`, not on the
message. `rest_forbidden` arrives with HTTP **401** even where it means "authenticated but not
permitted". Full catalogue: `errors/virtual-incision-problem-types.yml`.
