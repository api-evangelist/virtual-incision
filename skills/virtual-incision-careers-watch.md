---
name: virtual-incision-careers-watch
description: Read Virtual Incision job openings and their category, job-type and location taxonomies as JSON, for hiring-signal tracking on a surgical robotics company.
api: virtual-incision:virtual-incision-careers-api
generated: '2026-09-04'
method: generated
source: openapi/virtual-incision-careers-api-openapi.yml
operations:
  - getJobpost
  - getJobpostById
  - getJobpostCategory
  - getJobpostLocation
  - getJobpostJobType
---

# Read Virtual Incision job openings

The careers page at `https://virtualincision.com/careers/` is backed by a `jobpost` WordPress custom
post type with four taxonomies. All five collections are anonymously readable.

**Base URL:** `https://virtualincision.com/wp-json`
**Auth:** none for reads.

## Openings

`getJobpost` — `GET /wp/v2/jobpost?per_page=100&_fields=id,date,slug,link,title,jobpost_category,jobpost_location,jobpost_job_type`

**An empty array is a real answer.** At the last profile `X-WP-Total` was `0` — no openings were
published. Do not treat that as an error or retry it; record the zero.

## Taxonomies — the durable signal

The term lists persist even when no opening is live, and they describe how the company organises
hiring:

- `getJobpostCategory` — `GET /wp/v2/jobpost_category?per_page=100&_fields=id,name,slug,count` (9 terms observed)
- `getJobpostLocation` — `GET /wp/v2/jobpost_location?per_page=100&_fields=id,name,slug,count` (8 terms observed, e.g. `Lincoln, NE`, `Delafield, WI`, `Delafield, WI -or- Lincoln, NE`)
- `getJobpostJobType` — `GET /wp/v2/jobpost_job_type?per_page=100&_fields=id,name,slug,count`

`count` is the number of published postings carrying the term — a cheap way to see where hiring is
concentrated without fetching every posting.

## One opening in detail

`getJobpostById` — `GET /wp/v2/jobpost/{id}`

`content.rendered` is HTML. An `acf` object is present on this type (Advanced Custom Fields); its
inner shape is not published in the item schema, so read it defensively and never assume a key.

## Rules

- Read-only. Every `create*`, `update*` and `delete*` operation in this spec requires a WordPress
  Application Password and will return `rest_forbidden` (HTTP 401) to you. Do not attempt them.
- Same pagination, caching and courtesy rules as `virtual-incision-newsroom-monitor`.
