# Global Crude Flow Intelligence — daily data refresh (this repo)

This repository is a static website (GitHub Pages) showing a global crude
oil flow dashboard: production → export infrastructure → loading →
destination → disruption → replacement. `index.html` reads its data from
`data.json` in this same repo at runtime — nothing else in this repo needs
to change for a normal data update.

You are run here once a day (GitHub Actions cron) to check for and apply
real changes to `data.json`, then push the update straight to this repo.
This is an unattended run — there is no one to ask questions; make
reasonable judgment calls, and finish in one pass: check → (edit
`data.json` only if something real changed) → commit → push.

**No true real-time news feed is connected here** (free/public sources
only, no Kpler/Vortexa/EIA API access) — daily polling is the closest
practical approximation to "live" that's achievable this way. Some days
will find nothing materially new, and that's expected, not a failure.

## Keep it fast and cheap — this is not an unbounded deep-dive every time

Doing a full multi-region research sweep with no prioritisation every
single run would be slow, expensive, and mostly redundant. Instead:

1. **Fast check first (every run).** A handful of targeted searches only:
   - Anything in the current `disruptions` list with `status` containing
     "ongoing"/"unresolved"/"unclear" — has it resolved, escalated, or
     changed materially since yesterday?
   - A quick headline sweep (2-3 searches) for brand-new disruptions,
     force-majeure declarations, attacks, sanctions, substitution stories,
     or tender/sourcing-shift news, prioritising Middle East/Hormuz,
     Russia/Baltic/CPC, Nigeria, Libya, Guyana, and Venezuela.
2. **Deep dive only if the fast check turns up something real.** If you
   find a genuine new development or a real change to a tracked item, then
   (and only then) dig further: check `weekly_flows` rows with
   `confidence: "LOW"` for a possible better source, read the specific
   route/country's fuller context, and update `data.json` properly per the
   schema below.
3. **If the fast check finds nothing new:** don't force an edit. Just
   update `meta.generated` to the current timestamp (see below), commit,
   and push, so the site's "last checked" time stays honest and current.
   Do not touch any other field just to have something to report.

## Conventions — do not change without being told to

- **Weeks run Monday → Sunday.** `meta.week_ending` is always that Sunday's
  ISO date (`YYYY-MM-DD`) — the Sunday that ends the current week. If
  today is Europe/Nicosia-Sunday and it's before 09:00 there, that Sunday
  is still the current `week_ending`; once past 09:00 Nicosia on Sunday
  (or any day Monday-Saturday), `week_ending` is the *next* upcoming
  Sunday. `meta.cutoff` stays `"09:00 Europe/Nicosia"` and
  `meta.week_convention` keeps explaining that framing — it governs how
  headline weekly figures are grouped, not how often checks run.
- **`meta.generated` is a full timestamp, not just a date** — set it to
  the current time as ISO 8601 UTC, e.g. `2026-09-16T14:00:12Z`, every
  single run (whether or not anything else changed). This is what the
  site's "Live · updated …" indicator displays, down to the minute.
- **Confidence tagging.** Every flow/disruption figure carries a
  `confidence` of `HIGH`, `MEDIUM`, or `LOW`. Never invent a precise number
  you don't have a real source for — leave `mbd: null` with an honest
  `event_explanation` instead. Never collapse production capacity, actual
  exports, loading, destination, delivery, and refinery intake into one
  figure; keep them conceptually distinct even when only one is known.
- Never reintroduce a green/red-only status pair without shape-coding — but
  you won't be touching the map/status-colour code in a normal data
  refresh, only `data.json`'s content, so this is just a heads-up if you
  ever do touch `index.html`.

## `data.json` structure

Top-level keys: `meta`, `countries`, `infrastructure`, `weekly_flows`,
`disruptions`, `cargo_substitution`, `tenders`, `change_log`. Read the
current file first to see the exact shape of each row before editing —
don't guess field names.

- `weekly_flows` rows: each tracks one route/system. On a real change, move
  the current `mbd` into `previous_week_mbd`, set the new `mbd`, recompute
  `change_mbd` and `change_percent`, and update `status`,
  `event_explanation`, `confidence`, `source`, `source_date`. Only touch a
  row if the change is genuinely new information — a new source, a real
  move >5% or >100 kb/d, a new disruption, an outage, a new terminal, a
  cancelled cargo, a destination change, new replacement supply, or a
  major refinery change — never re-edit a row just because a day passed.
- `disruptions`: append new rows for new events. Update existing rows'
  `status`/`actual_restart` when something resolves or escalates. Keep
  resolved disruptions in the list with `status` updated (e.g.
  `"Restored"`) rather than deleting them.
- `cargo_substitution`: append new rows as new substitution stories are
  confirmed (never remove or fabricate). This is market-level replacement
  flow (a whole trade lane), distinct from `tenders` below.
- `infrastructure[].status` / `infrastructure[].map_status`
  (`Green`/`Amber`/`Red`/`Blue`) — update for any route whose situation
  changed. This also drives the dashboard's Hotspots tab automatically
  (grouped by `infrastructure[].region`, shown whenever a region has ≥1
  non-Green route) — no separate config to maintain there.
- `tenders`: append a row whenever you find a named buyer's discrete
  purchase/tender decision — who's buying, what grade, from where, and
  (only if a source actually disclosed it) volume/price basis. Fields:
  `tender_id`, `date`, `buyer`, `buyer_country`, `volume`, `crude_grade`,
  `origin_country`, `origin_previous`, `price_basis`, `status`,
  `confidence`, `source`, `source_date`, `note`. Never invent a
  volume/discount a source didn't state — write `"Not disclosed"` instead.
- `change_log`: append one row for **every** substantive edit made this
  run (a corrected figure, a new/updated disruption, a new tender, a
  status change) — this feeds the dashboard's Changes tab (day/week/month
  view). Fields: `change_id` (increment from the highest existing),
  `timestamp` (full ISO 8601 UTC, same as this run's `meta.generated`),
  `date`, `category` (e.g. "Correction", "New disruption", "New tender
  data", "Resolved"), `scope`, `summary`, `detail`, `confidence`, `source`.
  If nothing changed this run, don't add a row just to have one.

## What to do, step by step

1. Read the current `data.json` in this repo.
2. Run the fast check (above). If it finds nothing material, skip to step
   5.
3. If the fast check found something real, do the deeper research it
   needs — free/public sources only: Reuters, Al Jazeera, Bloomberg,
   OilPrice.com, Argus, S&P Global, IEA, OPEC, EIA, national operator/NOC
   statements.
4. Edit `data.json` in place with the changes, following the schema and
   confidence rules above (including `tenders`/`change_log` where
   applicable). Keep the file valid JSON (check it parses).
5. Always set `meta.generated` to the current ISO 8601 UTC timestamp, then
   commit (message like `Data refresh — 2026-09-16T14:00Z` — or note what
   changed if something did) and push, using git directly:
   `git add data.json && git commit -m "..." && git push`. Do not touch
   any other file in this repo during a normal refresh.

Do not wait for approval or ask a question — this is a scheduled,
unattended run.
