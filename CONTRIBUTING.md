# Contributing to PSOBB Journey Log

Thanks for wanting to help. The project is intentionally small and hackable — one HTML file, one JavaScript array of goals. Contributions are usually a dozen lines of JSON-ish data.

## Ways to contribute

1. **Suggest a goal via issue** — no code needed. Use the "New Goal" issue template.
2. **Submit a goal via PR** — edit the `GOALS_DATA` array in `index.html`.
3. **Report a bug** — open an issue with the "Bug Report" template.
4. **Fix a bug or add a feature** — PRs welcome, but please open a discussion issue first for anything beyond a small fix so we don't duplicate effort.

## The goal schema (v3)

Every goal is an object in the `GOALS_DATA` array near the top of the `<script>` block in `index.html`. Required and optional fields:

```js
{
  id: 'h-fo-psycho',              // required, unique, kebab-case. Conventional prefixes:
                                  //   g-    = global
                                  //   c-    = character (generic)
                                  //   t-    = character techs
                                  //   h-    = character hunts
                                  //   ch-   = challenge run
                                  //   col-  = collection set
                                  //   pb-   = performance / speedrun PB
  scope: 'character',             // required. 'global' or 'character'
  kind: 'progression',            // required (v3). See "Kinds" below.
  tier: 'legendary',              // required. 'core' | 'stretch' | 'legendary'
  category: 'Hunts',              // required. Free-text, grouped in UI. Keep consistent with existing ones.
  title: 'Psycho Wand',           // required. Short, imperative or noun-phrase.
  desc: 'Dark Falz Ultimate drop on specific IDs.',  // optional but strongly encouraged
  classes: ['FOnewm', 'FOmar', 'FOmarl', 'FOnewearl'], // optional. Array of class names, or 'all'.
                                  //                     Required only when scope is 'character' and not all classes apply.
  server: 'ephinea',              // optional. 'any' (default) | 'ephinea' | 'schtserv' | 'ultima'
  episode: 'Ep2',                 // optional. Cosmetic tag for filtering later.

  // --- SEASONAL-only fields ---
  eventDates: {                   // required if kind is 'seasonal'
    months: [8],                  // array of 1-12 month numbers when the event is live (wrap if it spans Dec-Jan)
    label: 'August',              // short human label ("February", "December")
    recurring: true               // does it run every year? set false for one-off events.
  },

  // --- PERFORMANCE-only fields ---
  repeatable: true,               // optional. Marks a goal as re-logged with attempts instead of checked-off once.
  track: 'time'                   // optional. 'time' = mm:ss personal best. If omitted on repeatable,
                                  //           Log Attempt becomes a simple attempt counter with notes.
}
```

## Kinds

The `kind` field groups goals by what _kind_ of play they reward. The UI has a kind filter bar so players can focus on what they care about today. Pick one:

| Kind          | What it's for                                                             | Example                                      |
|---------------|---------------------------------------------------------------------------|----------------------------------------------|
| `progression` | Leveling, unlocks, beating content for the first time                     | "Reach Level 100", "Defeat Olga Flow on VH"  |
| `challenge`   | Self-imposed constraint runs                                              | "No-mag VH clear", "Solo Ult Falz at Lv100-" |
| `seasonal`    | Events that only run during certain months                               | "Participate in the Anniversary Event"       |
| `collection`  | Gotta-catch-em-all sets — all elements, all bosses, all rappies, all IDs | "Evolve 3 distinct rare mags"                |
| `performance` | Speedruns, personal bests, re-runnable skill tests                        | "Towards the Future PB"                      |
| `meta`        | About using the tracker itself, team, community, mentorship             | "Log 50 milestones", "Fill your Team to 10" |

Default: if you truly can't decide, `progression` is the safest bet, but try to place it more specifically.

## Tiers

Pick honestly:

- **`core`** — 90%+ of active players will hit this. Baseline vocabulary.
- **`stretch`** — focused project territory. A week to a month of deliberate play.
- **`legendary`** — months to years. Single-digit % of players. White whales.

## Class names (exact casing)

```
HUmar  HUnewearl  HUcast  HUcaseal
RAmar  RAmarl     RAcast  RAcaseal
FOmar  FOmarl     FOnewm  FOnewearl
```

Class families used in the current library:

- `HUNTERS` — all four HU classes
- `RANGERS` — all four RA classes
- `FORCES`  — all four FO classes
- `TECH_CAPABLE` — all FOs + HUmar, HUnewearl, RAmar, RAmarl (non-android, tech-learning)

When you need a class subset, either use one of these constants or list the classes explicitly.

## Writing a seasonal goal

Seasonal goals only surface while the event is live (or coming soon). Past events hide by default unless the player toggles "Show past events". So `eventDates.months` is what drives visibility:

```js
{
  id: 'g-event-halloween',
  scope: 'global',
  kind: 'seasonal',
  tier: 'core',
  category: 'Events',
  title: 'Participate in Halloween Event',
  desc: 'November. Halloween Cookie drops from all monsters on all difficulties.',
  server: 'ephinea',
  eventDates: { months: [11], label: 'November', recurring: true }
}
```

If an event spans two months (e.g. mid-December through early January), list both: `months: [12, 1]`.

## Writing a performance / PB goal

Performance goals are repeatable. The player logs attempts over time, and if `track: 'time'`, the tracker records a personal best in mm:ss:

```js
{
  id: 'pb-ttf',
  scope: 'character',
  classes: 'all',
  kind: 'performance',
  tier: 'stretch',
  category: 'Speedruns',
  title: 'Towards the Future PB',
  desc: 'Log your best solo time. Beatable forever.',
  server: 'any',
  repeatable: true,
  track: 'time'
}
```

A repeatable goal without `track: 'time'` still works — it just counts attempts with notes instead of tracking a time.

## Writing a challenge-run goal

Challenge runs are self-imposed constraints. They should be crisp enough that a player can honestly answer "did I do this?" without ambiguity:

```js
{
  id: 'ch-no-mag',
  scope: 'character',
  classes: 'all',
  kind: 'challenge',
  tier: 'stretch',
  category: 'Challenge Runs',
  title: 'Clear a VH quest with no mag equipped',
  desc: 'No Photon Blast safety net. No auto-stats.',
  server: 'any'
}
```

Avoid "impressive-sounding but impossible to verify" framing ("defeat Falz stylishly"). Keep constraints mechanical.

## Style guide for goals

**Do:**

- Write titles that stand on their own. `"Reach Level 80"` — good. `"Level goal"` — bad.
- Add a `desc` that explains _why this matters_ or a useful note — a drop location, a significance, a tactical tip.
- Use consistent category names. Current categories include `Progression`, `Quests`, `Bosses`, `Mag`, `Techs`, `Hunts`, `Team`, `Community`, `Collection`, `Economy`, `Events`, `Challenge Runs`, `Speedruns`, `Modes`, `Meta`.
- Tag server-specific goals with `server: 'ephinea'` (or whichever). If it works across all BB variants, omit or use `'any'`.
- Pick a tier honestly. If 90%+ of active players achieve it, it's `core`. If it takes a focused project, it's `stretch`. If it's a months-to-years endeavor or single-digit % of players, it's `legendary`.
- Pick the most specific `kind` that fits.

**Don't:**

- Duplicate existing goals. Search `GOALS_DATA` first.
- Create goals that can't be self-verified ("be in the top 100 players"). If a player can't honestly tell whether they've done it, it's not a good goal.
- Create joke goals that clutter the library. Submit those as custom-goal examples in docs instead.
- Add per-level goals for every level between 80 and 200. The current pacing (50, 80, 100, 150, 180, 200) is deliberate.
- Make seasonal goals that aren't actually tied to a specific month — those belong in other kinds.

## Adding how-to guidance (strategy, wiki link, prerequisites)

Some goals — especially rare hunts and unseal chains — need background knowledge to be actionable. The tracker supports a **How-to panel** on each card that surfaces three things:

- `strategy` — a one-line tip on how to approach the goal
- `wiki` — a link to the Ephinea wiki entry (or any other reference)
- `requires` — an array of goal IDs that should be done first (shown as a checklist that deep-links to those cards)

These fields live in a **separate lookup table** called `GOAL_STRATEGY`, not on the goal objects themselves. This keeps `GOALS_DATA` minimal and lets contributors add strategy notes without touching existing goals.

### Shape

```js
const GOAL_STRATEGY = {
  'h-ra-snow': {
    strategy: 'Equip Frozen Shooter and kill 30k enemies to unseal it into Snow Queen.',
    wiki: 'https://wiki.pioneer2.net/en/Snow_Queen',
    requires: ['h-ra-frozen']
  },
  // ...
};
```

All three fields are optional. The panel only renders if the goal has at least one of `strategy`, `wiki`, or `requires`.

### Adding a strategy entry

1. Find (or add) `GOAL_STRATEGY` near the bottom of `GOALS_DATA` in `index.html`.
2. Add a key matching the goal's `id`.
3. Fill in whichever of the three fields apply. Keep `strategy` to ~1 line; if it needs more, it probably belongs on the wiki.

### Chaining prerequisites

If goal B can only be completed after goal A, add `requires: ['A-id']` to B's strategy entry. The How-to panel will render a checklist — green check if A is achieved, empty box if not. Clicking a prerequisite scrolls to its card and flashes it.

Good examples of chains in the current library:

- `h-ra-snow` (Snow Queen) requires `h-ra-frozen` (Frozen Shooter)
- `g-own-psycho` (Own a Psycho Wand) requires `h-fo-psycho` (the hunt)
- `g-own-jsword` (Own an unsealed J-Sword) requires `h-hu-jsword` (the unseal)

Don't over-chain. A prereq should be a real mechanical gate, not "this would help." If someone can buy/trade their way past the prereq, it isn't a prereq.

### Style notes

- **Strategy lines**: concrete and actionable. "Dark Falz Ult, IDs Skyly/Bluefull/Purplenum/Pinkal" beats "rare FO weapon, good luck".
- **Wiki links**: prefer Ephinea wiki (`https://wiki.pioneer2.net/en/…`) since most seasonal/drop info is server-specific. Other wikis are fine when no Ephinea page exists.
- **Don't mirror the wiki**. One line pointing the reader in the right direction, plus the link. Not a full guide.

## Submitting a PR

1. Fork the repo.
2. Add your goal(s) to `GOALS_DATA` in `index.html`. Group near similar goals for readability.
3. Open the file in your browser (just double-click `index.html`) and sanity-check that your goal shows up in the right tier/scope/class/kind combination.
4. If it's a seasonal goal, try toggling "Show past events" to confirm the status badge (active / upcoming / past) looks right for the current month.
5. Open a PR with a title like `Add goal: Hunt — Heaven Striker`. In the description, note the server if applicable and which class family it targets.

Small PRs (1–5 goals) will usually get reviewed faster than large ones.

## Suggesting via issue instead

If editing JS is not your thing, just open an issue using the "New Goal" template. A maintainer (or another contributor) will turn it into a PR.

## Bigger changes

For UI changes, new tier concepts, adding a new `kind`, or reworking the data model, please open a discussion issue first. Drive-by redesign PRs on a personal-use project rarely merge — not out of gatekeeping, but because the codebase stays hackable by staying simple.

## Code of conduct

Be decent. This is a tracker for a 25-year-old online game. Perspective is free.
