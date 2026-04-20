# PSOBB Journey Log

A single-file, offline-first goal tracker for **Phantasy Star Online Blue Burst**. Track milestones across your whole account and each individual character — from your first boss kill to your tenth-year Psycho Wand.

> *PSO has no built-in achievement system. This is one.*

## Try it live

**[→ Open the demo](https://jjmasse.github.io/pso-bb-goal-tracker/)**

The demo runs entirely in your browser. Your data stays in your browser's `localStorage` — nothing is sent anywhere, and no one else can see your progress. Use the demo to kick the tires. For real long-term tracking, fork or download and run `index.html` locally — that way your log survives a repo move and isn't tied to the hosted URL.

> **Forking?** If you publish your own GitHub Pages demo, change the demo link above to `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/`. See [hosting your own copy](#hosting-your-own-copy) below.

## What it is

- **Multi-character**: manage any number of characters, each with their own class, Section ID, and progression history.
- **Two scopes**:
  - **Global** — account-wide goals (Team milestones, rare collections, lifetime Photon Drops, event participation).
  - **Character** — per-character goals, automatically filtered by class (no Force tech goals on your HUcast).
- **Three tiers**: Core Path · Stretch Goals · Legendary. Layered so you can focus without losing sight of long-term dreams.
- **Journey log framing**: no looming completion percentages. Recent milestones surface in a feed — the dashboard celebrates what you've *done*, not what's left.
- **Notes per achievement**: attempt count, party composition, Section ID at time of drop, memorable moments.
- **Server-aware**: default data targets **Ephinea**. Server-specific goals are tagged and filterable.
- **Custom goals**: add your own per-character or global goals that won't affect the shared library.
- **Local-first**: state lives in your browser's `localStorage`. Export/import JSON for backup or to move between devices.

## How to use

1. Download `index.html` (or clone the repo).
2. Double-click the file. It opens in your default browser.
3. Click **Manage Characters** to add your first character.
4. Toggle between Global view and character views with the dropdown at the top.

No install, no server, no account. The file works offline. Works in any modern browser.

## Screenshots

*(Add screenshots here after first run — drop PNGs in a `docs/screenshots/` folder and link them in place of this line.)*

## What this is *not*

- **Not** a PSO launcher, save editor, or game modification tool. It never touches your game files.
- **Not** connected to any server's API. Achievements are self-reported.
- **Not** opinionated about which server you play on — Ephinea is the default because of its active community, but you can toggle the server filter to view all goals.

## Hosting your own copy

The repo is set up to auto-deploy to GitHub Pages on every push to `main`. If you fork it and want your own live demo:

1. Fork the repo (or push the files to a new one of your own).
2. Go to **Settings → Pages**.
3. Under **Build and deployment → Source**, select **GitHub Actions**.
4. Push any commit (or manually run the **Deploy to GitHub Pages** workflow from the Actions tab).
5. Your demo will be live at `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/` within a minute or two.

The included workflow is at [.github/workflows/pages.yml](.github/workflows/pages.yml). It publishes the repo root as-is — no build step, just static files.

If you'd rather not use GitHub Actions, you can also enable Pages via **Settings → Pages → Deploy from a branch → main → `/`**. Same result.

## Contributing

Goals and bug reports are welcome. The goal library is embedded in `index.html` as a JavaScript array (`GOALS_DATA`). See [CONTRIBUTING.md](CONTRIBUTING.md) for the schema and how to submit a new goal.

Class libraries are intentionally uneven in v1 — Force content is most detailed because that's what the original author mains. Ranger and Hunter libraries especially welcome community contributions.

## Roadmap ideas

Nothing promised. Candidates for future versions:

- Session log view (what you did in a play session, time-stamped)
- Team-scoped goals (shared across members)
- Per-character quest checklist with difficulty matrix
- Enemy encyclopedia tracking
- Import/export of just the goal library for server-specific forks
- Optional completion percentage view (for people who *want* the nag)

## License

[MIT](LICENSE) — use, modify, fork freely.

## Credits

Inspired by the Ephinea community and a decade of people grinding for weapons that may never drop.
