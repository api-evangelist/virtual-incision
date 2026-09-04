---
name: virtual-incision-media-assets
description: Locate Virtual Incision MIRA imagery and press assets through the media library API and resolve the direct source URL and MIME type for each one.
api: virtual-incision:virtual-incision-media-api
generated: '2026-09-04'
method: generated
source: openapi/virtual-incision-media-api-openapi.yml, openapi/virtual-incision-pages-api-openapi.yml
operations:
  - getMedia
  - getMediaById
  - getPages
---

# Find Virtual Incision media assets

249 media items were anonymously readable at the last profile. This is the machine-readable route to
MIRA imagery and press assets.

**Base URL:** `https://virtualincision.com/wp-json`
**Auth:** none for reads.

## List

`getMedia` — `GET /wp/v2/media?per_page=100&_fields=id,slug,link,media_type,mime_type,source_url,alt_text`

- `source_url` is the direct file URL (`https://virtualincision.com/wp-content/uploads/...`).
- `media_type` is `image` / `file` / `video`; `mime_type` is exact.
- Filter by `search=`, by `media_type=`, or by `parent=<post id>` to get the assets attached to one
  press release.

## Detail

`getMediaById` — `GET /wp/v2/media/{id}`

`media_details` carries the generated size variants with width, height and file name for each — use
it to pick a size rather than guessing a suffix.

## Context

`getPages` — `GET /wp/v2/pages?search=media&_fields=id,slug,link,title`

The formal request route is the human page `https://virtualincision.com/request-media-kit/`. For any
use beyond reference, go through it: these are a medical-device company's regulated product images.

## Rules

- **Never** call `createMedia`, `updateMediaById` or `deleteMediaById`. They are authenticated
  operations, and `deleteMediaById` is the one write on this host with **no reversal at all** —
  WordPress media deletion is always permanent. See the `reversibility` block in
  `conventions/virtual-incision-conventions.yml`.
- Respect `Crawl-delay: 10` from `robots.txt` when walking 249 items.
