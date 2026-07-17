# StremioLens

Client-side library explorer for [Stremio](https://www.stremio.com/) accounts.

Authenticates against the public Stremio API, loads the signed-in user’s library, and enriches titles with Cinemeta metadata (year, IMDb rating, genres). Runs entirely in the browser from a single static page. Credentials are never persisted.

> **Disclaimer:** Unofficial project. Not affiliated with, endorsed by, or maintained by Stremio. Stremio® is a trademark of its respective owners.

---

### Live application

**[Open StremioLens on GitHub Pages →](https://lcarlini.github.io/StremioLens/)**

Hosted from this repository’s `index.html` via GitHub Pages (`main` / root).

---

## Overview

| | |
|---|---|
| **Entry point** | [`index.html`](./index.html) |
| **Runtime** | Browser only (no backend, no build step) |
| **Auth** | Stremio account email + password (same as the official apps) |
| **APIs** | `api.strem.io`, `cinemeta-live.strem.io` |

## Requirements

- Modern browser with `fetch` and ES2020 support
- Network access to Stremio and Cinemeta endpoints
- A valid Stremio account
- HTTPS (or `localhost`) — do not use `file://`

## Architecture

```
Browser (index.html)
  │
  ├─ POST https://api.strem.io/api/login
  │     → authKey (memory only)
  │
  ├─ POST https://api.strem.io/api/datastoreGet
  │     → libraryItem[]
  │
  └─ GET  https://cinemeta-live.strem.io/meta/{type}/{id}.json
        → year, imdbRating, genres
```

| Layer | Behavior |
|-------|----------|
| Authentication | Stremio `Login`; password cleared from the DOM after success |
| Storage | No credential writes to `localStorage`, `sessionStorage`, cookies, or disk |
| Enrichment | Concurrent Cinemeta requests for `movie` and `series` |
| Export | Client-generated CSV download |

## Capabilities

- Library load (movies, series, other)
- Metadata enrichment (year, ranking, genres)
- Filters: search, type, watched, genre, year range, minimum ranking
- Sortable table and aggregate stats
- CSV export and IMDb deep links

## Local development

```bash
git clone https://github.com/lcarlini/StremioLens.git
cd StremioLens
npx --yes serve .
```

Open the local URL printed by the server.

## GitHub Pages

Deployed from branch `main`, folder `/ (root)`.

| Setting | Value |
|---------|--------|
| Source | Deploy from a branch |
| Branch | `main` |
| Folder | `/ (root)` |
| URL | https://lcarlini.github.io/StremioLens/ |

No CI build is required; Pages serves `index.html` directly.

## Security

| Policy | Detail |
|--------|--------|
| No credential persistence | Email/password are never stored |
| Session scope | `authKey` exists only in memory until sign-out or tab close |
| Transport | Prefer HTTPS for login payloads |
| Data plane | Traffic limited to Stremio API and Cinemeta |

Use only deployments you trust when entering account credentials.

## API references

- Stremio API: `https://api.strem.io`
  - `POST /api/login`
  - `POST /api/datastoreGet` (`collection: libraryItem`)
- Cinemeta: `https://cinemeta-live.strem.io` ([addon docs](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/advanced.md))
- Official products: [stremio.com](https://www.stremio.com/) · [web.stremio.com](https://web.stremio.com/)

## Repository layout

```
StremioLens/
├── index.html
├── README.md
├── LICENSE
└── .gitignore
```

## License

See [LICENSE](./LICENSE).
