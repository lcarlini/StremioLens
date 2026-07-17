<div align="center">

# StremioLens

**A client-side library explorer and discover tool for [Stremio](https://www.stremio.com/) accounts.**

[![Live Demo](https://img.shields.io/badge/demo-GitHub%20Pages-ff5a36?style=for-the-badge&logo=githubpages&logoColor=white)](https://lcarlini.github.io/StremioLens/)
[![License](https://img.shields.io/badge/license-see%20LICENSE-3ec6ff?style=for-the-badge)](./LICENSE)
[![Stack](https://img.shields.io/badge/stack-single%20index.html-07090d?style=for-the-badge&logo=html5&logoColor=white)](./index.html)
[![API](https://img.shields.io/badge/API-Stremio%20%2B%20Cinemeta-5ddea5?style=for-the-badge)](https://api.strem.io/)
[![i18n](https://img.shields.io/badge/lang-EN%20%7C%20PT%20%7C%20ES-8b9bb0?style=for-the-badge)](#features)
[![Responsive](https://img.shields.io/badge/UI-PC%20·%20Tablet%20·%20Phone-ff8a3d?style=for-the-badge)](#features)

**[Open StremioLens →](https://lcarlini.github.io/StremioLens/)**

*No install · No backend · No credential storage*

</div>

---

> **Disclaimer:** StremioLens is an **unofficial** project. It is **not affiliated with, endorsed by, or maintained by Stremio**. Stremio® is a trademark of its respective owners.

---

## About

StremioLens authenticates against the public **Stremio API**, loads your signed-in library, browses **Cinemeta** catalogs (Popular, New/by year, Featured), searches titles, opens rich details (poster, cast, trailers), and can **add or remove** items in your Stremio library via `datastorePut`.

Everything runs **entirely in the browser** from a single static page — deployed on **GitHub Pages** with zero build step and zero server-side code. Your email and password are **never persisted**; the session `authKey` lives in memory only until you sign out or close the tab.

| | |
|---|---|
| **Repository** | [github.com/lcarlini/StremioLens](https://github.com/lcarlini/StremioLens) |
| **Live app** | [lcarlini.github.io/StremioLens](https://lcarlini.github.io/StremioLens/) |
| **Entry point** | [`index.html`](./index.html) |
| **Runtime** | Browser only — no backend, no build step |
| **Auth** | Stremio account email + password (session only) |
| **APIs** | `api.strem.io`, `cinemeta-live.strem.io`, `v3-cinemeta.strem.io` |

---

## Author

| | |
|---|---|
| **Name** | Leandro Carlini Mingorance |
| **Role** | Computer Engineer |
| **Portfolio** | [lcarlini.github.io/lcarlini](https://lcarlini.github.io/lcarlini/) |
| **GitHub** | [github.com/lcarlini](https://github.com/lcarlini) |

StremioLens was designed and implemented by **Computer Engineer Leandro Carlini Mingorance**.

---

## Features

### Responsive layout

Optimized for **desktop**, **tablet**, and **smartphone** — adaptive grids, toolbars, and modals that stay usable on any screen size.

### Internationalization (i18n)

Built-in UI translations with a language selector:

| Language | Flag |
|----------|------|
| **English** *(default)* | 🇺🇸 EN |
| **Portuguese** | 🇧🇷 PT |
| **Spanish** | 🇪🇸 ES |

Language selector uses SVG flags + language codes in the app header.

Language preference is remembered in the browser (`localStorage` key `stremiolens_lang` only — never credentials).

### Library

- Load movies, series, and other items from your Stremio account
- Filter and sort your collection
- **Statistics** panel — counts by type, genre, decade, and more
- **CSV export** of the filtered library

### Views

Switch between three presentation modes:

| View | Description |
|------|-------------|
| **Poster grid** | Visual cover-based browsing |
| **Table** | Sortable rows with cover thumbnails |
| **List** | Compact rows for quick scanning |

### Discover

Browse Cinemeta catalogs without leaving the app:

- **Popular** — trending titles
- **New** — releases filtered by year
- **Featured** — IMDb-curated picks

### Search

Full catalog search for any movie or series across Cinemeta.

### Details modal

Rich metadata for any title — poster, description, cast, director, trailer embed, and IMDb link.

### Torrentio streams

Open **Torrentio links** from a title’s details modal:

- Loads streams from `torrentio.strem.fun`
- Filter by **quality**, **source**, and **min seeders**
- **Copy magnet** or **Open / download** (magnet)
- Copy/open direct URLs when present
- Series: choose season & episode

> Unofficial Torrentio integration. StremioLens does not host files. Use only content you have rights to access.

### Library write

Add titles to or remove them from your Stremio library via `datastorePut` — changes sync to your official Stremio account.

---

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
  ├─ POST https://api.strem.io/api/datastorePut
  │     → add / remove library items
  │
  ├─ GET  https://v3-cinemeta.strem.io/catalog/{type}/{id}[/{extras}].json
  │     → Popular / New(year) / Featured / search
  │
  └─ GET  https://cinemeta-live.strem.io/meta/{type}/{id}.json
        → poster, year, rating, genres, cast, trailers
```

---

## Requirements

- Modern browser with `fetch` and ES2020 support
- Network access to Stremio and Cinemeta endpoints
- A valid Stremio account
- **HTTPS** (or `localhost`) — do not open via `file://`

---

## Local development

```bash
git clone https://github.com/lcarlini/StremioLens.git
cd StremioLens
npx --yes serve .
```

Then open the URL printed by the static server (typically `http://localhost:3000`).

---

## GitHub Pages deployment

| Setting | Value |
|---------|--------|
| Source | Deploy from a branch |
| Branch | `main` |
| Folder | `/ (root)` |
| URL | [https://lcarlini.github.io/StremioLens/](https://lcarlini.github.io/StremioLens/) |

---

## Security

| Policy | Detail |
|--------|--------|
| **No credential persistence** | Email and password are never stored in `localStorage`, cookies, or any other client storage |
| **Session scope** | `authKey` exists only in memory until sign-out or tab close |
| **Transport** | Use HTTPS for login and library writes |
| **Data plane** | Traffic is limited to the Stremio API and Cinemeta endpoints |

---

## API references

| Service | Base URL | Endpoints |
|---------|----------|-----------|
| Stremio API | `https://api.strem.io` | `login`, `datastoreGet`, `datastorePut` |
| Cinemeta catalogs | `https://v3-cinemeta.strem.io` | Catalog JSON ([manifest](https://v3-cinemeta.strem.io/manifest.json)) |
| Cinemeta meta | `https://cinemeta-live.strem.io` | Title metadata JSON |

Official Stremio products: [stremio.com](https://www.stremio.com/) · [web.stremio.com](https://web.stremio.com/)

---

## Repository layout

```
StremioLens/
├── index.html      # Single-page app (HTML, CSS, JS)
├── README.md
├── LICENSE
└── .gitignore
```

---

## License

See [LICENSE](./LICENSE).
