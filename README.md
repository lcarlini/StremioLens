# StremioLens

Unofficial client-side tool to inspect a [Stremio](https://www.stremio.com/) library in the browser.

Sign in with your **Stremio account** (same email/password as the Stremio app). Credentials are used only to call Stremio’s public API for the current session and are **never stored**.

> StremioLens is not affiliated with, endorsed by, or maintained by Stremio. Stremio® is a trademark of its respective owners.

## Requirements

| Item | Notes |
|------|--------|
| Browser | Modern Chromium, Firefox, or Safari with `fetch` + ES2020 |
| Network | Access to `api.strem.io` and `cinemeta-live.strem.io` |
| Account | Valid Stremio account (email + password) |
| Hosting | HTTPS recommended (GitHub Pages or local static server) |

Do **not** open `index.html` via `file://` — browsers typically block cross-origin API calls from local files.

## Architecture

Single static page: [`index.html`](./index.html) (HTML + CSS + JS, no build step).

```
Browser (index.html)
  │
  ├─ POST https://api.strem.io/api/login
  │     → authKey (in-memory only)
  │
  ├─ POST https://api.strem.io/api/datastoreGet
  │     → libraryItem collection
  │
  └─ GET  https://cinemeta-live.strem.io/meta/{type}/{id}.json
        → year, imdbRating, genres
```

| Concern | Implementation |
|---------|----------------|
| Auth | Stremio `Login` API; password cleared from the DOM after success |
| Persistence | None for credentials (`localStorage` / `sessionStorage` / cookies unused for auth) |
| Enrichment | Parallel Cinemeta requests (movies & series) |
| Export | Client-side CSV `Blob` download |

## Features

- Load library items (movies, series, other)
- Enrich with year, IMDb ranking, genres
- Filter: search, type, watched, genre, year range, min ranking
- Sortable table; stats (average ranking, by type, by decade, top genres)
- CSV export; open IMDb title pages

## Local development

```bash
git clone https://github.com/<user>/StremioLens.git
cd StremioLens

# any static file server works
npx --yes serve .
# or: python -m http.server 8080
```

Open the printed `http://localhost:…` URL.

## Deploy (GitHub Pages)

1. Push `main` to GitHub.
2. Repository **Settings → Pages**.
3. **Build and deployment → Source:** Deploy from a branch.
4. Branch: `main`, folder: `/ (root)`.
5. Site URL: `https://<user>.github.io/StremioLens/`

No CI build is required; Pages serves `index.html` as-is.

## Security & privacy

| Rule | Detail |
|------|--------|
| No credential storage | Email/password are never written to disk, `localStorage`, `sessionStorage`, or cookies |
| Session memory only | `authKey` lives in a JS variable until Sign out or tab close |
| Form hygiene | Password input is cleared immediately after a successful login |
| Transport | Prefer HTTPS so the Stremio login payload is encrypted in transit |
| Scope | Requests go only to Stremio API + Cinemeta; no third-party analytics |

Treat this page like any site where you type a password: only use trusted deployments you control.

## API references

- Stremio API host: `https://api.strem.io`
  - `POST /api/login` — authenticate Stremio account
  - `POST /api/datastoreGet` — read `libraryItem`
- Metadata: [Cinemeta](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/advanced.md) via `https://cinemeta-live.strem.io`

Official Stremio products: [stremio.com](https://www.stremio.com/) · [web.stremio.com](https://web.stremio.com/)

## Repository layout

```
StremioLens/
├── index.html      # application (UI + logic)
├── README.md       # this document
├── LICENSE
└── .gitignore
```

## License

See [LICENSE](./LICENSE).
