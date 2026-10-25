# Professionals — where cricket is emotion

A responsive cricket scorecard and performance dashboard built around the supplied C project. Professionals combines a **C REST server**, managed PostgreSQL persistence, a cinematic cricket-night interface, player rankings, match-history snapshots, and a cricket-aware Match Analyst.

## Run locally

Requirements: GCC, PostgreSQL client/development library (`libpq-dev`), and libcurl development headers/library (libcurl is used only when optional external AI is configured).

```bash
make
make test
DATABASE_URL="postgresql://USER:PASSWORD@HOST:5432/DATABASE?sslmode=require" ./scorecard_server
```

Open **http://localhost:8000** on a phone, tablet, or desktop. Set `PORT` to use another port. For a local Postgres container, use `docker compose up --build` (Docker required).

## Online deployment

The previous Vercel project was reported deleted by the owner. The current workspace updates are **not deployed**, and the status of the old public URL has not been re-verified. A new Vercel project will need to be created and connected to the C container and Neon PostgreSQL before release. The deployable source bundle is `professionals-vercel-app/`; see [`docs/DEPLOYMENT.md`](docs/DEPLOYMENT.md) before any release.

**Security:** you chose a public, editable link. There is no login, so anyone who reaches the public URL can edit or reset the scorecard. Use public/demo match data only; add authentication before storing private team data.

## Product features

- A visual-only Sign-In opening splash leads into five independent feature pages—**Overview**, **Performance**, **Scorecard**, **History**, and **Analyst**—with their own routes, scrolling wallpaper, sticky navigation, and browser back/forward support. The guest form is not authentication.
- Add, edit, and delete batter and bowler figures through the C API.
- Calculate runs, strike rates, innings run rate, legal-ball overs, economy, and leader records.
- Compare batters and bowlers in animated, benchmark-style charts, with a separate scroll-led Performance story, clearer high-contrast panels, and staggered chart/story motion.
- Archive point-in-time match summaries in PostgreSQL; snapshots retain the team names and batting-side label captured at archive time. Reset clears live player rows but preserves history and team settings.
- Edit Team A / Team B names and the manually selected batting side; these are shared global labels guarded by optimistic version checks and do not affect C scoring calculations.
- Dark, readable cricket-night interface using five user-supplied stadium images as section backgrounds and a slow crossfading photo carousel; reduced-motion preferences switch to a static image.
- Cinematic guest entry remains visual only: stages 1–2 use the stadium video and stage 3 uses the night-drone scene, now layered with the supplied arena-photo sequence; the player tag/role are local presentation only, not authentication. The available video loops remain 27 seconds/1280×674 and 14 seconds/1280×720—not a 30-second or 4K master.
- Distinct animated page scenes, a Player Pass, and live-progress Matchday Quests make the dashboard feel game-like while preserving the five existing pages.
- Match Analyst answers locally without an AI key; optional OpenAI-compatible AI is configured server-side.
- Load a demo scorecard for exploration; reset it when finished.
- The frontend polls the C API so changes from another open device/tab appear automatically.

## Optional AI API

Set these in the Vercel project environment or local server environment:

```text
AI_API_KEY=your-server-side-key
AI_API_URL=https://api.openai.com/v1/chat/completions
AI_MODEL=gpt-4o-mini
```

The browser never receives the API key. If the external call is unavailable or fails, Professionals falls back to its local cricket analyst. No external live-score feed is configured; scoring is manual.

## Project map

- `server.c` — C HTTP server, REST endpoints, PostgreSQL logic, local analyst, optional AI adapter.
- `index.html` — responsive multi-route frontend with independent feature pages, embedded CSS/JS, and no CDN requirement.
- `assets/` — local cinematic clips plus the five optimized user-supplied arena photos.
- `Makefile` — local build.
- `Dockerfile` — generic container build; `Dockerfile.vercel` — Vercel Container Runtime entrypoint.
- `compose.yaml` — local C server + PostgreSQL test stack.
- `professionals-vercel-app/` — linked Vercel deployment bundle with `Dockerfile.vercel`, explicit container-service routing, and production project metadata.
- `docs/` — product, API, architecture, database, deployment, security, testing, and setup notes.
