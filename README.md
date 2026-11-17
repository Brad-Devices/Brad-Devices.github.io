# Brad Devices — the site, second door

> The same public site as [brad-devices.vercel.app](https://brad-devices.vercel.app),
> served by GitHub Pages from this repository.

**Canonical:** https://brad-devices.vercel.app
**Mirror:** https://brad-devices.github.io

## Why a second host

A public front door that exists on exactly one platform account is one suspension,
one billing failure, or one region block away from being no front door at all. That
is the whole argument: not redundancy for its own sake, but the difference between a
company with a website and a company with a website on somebody's else's account.

It is also pragmatic. Some platforms refuse to accept links to shared hosting
subdomains. A second hostname is a fallback for exactly that class of problem.

## What is in here

The rendered site only — HTML, CSS, JS, the wordmark and icon assets, the
specification PDFs, `llms.txt`, `robots.txt`, `sitemap.xml`. The same payload that is
deployed to Vercel, and the same boundary: **no `src/`, no division documents, no
drivers.** The internal monorepo is not mirrored here, by policy.

## How it is served

`main` branch, repository root, no Jekyll (`.nojekyll`). The site's links are
root-absolute (`/css/style.css`, `/now`), which is why this is an **organisation
root site** — a project path like `/Brad-Devices.github.io/` would break every one of
them.

## Licence

Apache-2.0 for code, CC BY 4.0 for content. See `LICENSE`, `LICENSE-CONTENT` and
`NOTICE`. The names are trademarks; see the main repository's `TRADEMARKS.md`.

---

Brad Devices · Kisumu, Kenya
