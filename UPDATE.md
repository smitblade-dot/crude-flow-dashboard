# Updating your site with today's changes (2026-09-16)

Your automation is already set up and running daily — nothing to reconfigure. This is just a content update: five new/improved features, plus a data correction.

## What's new

1. **Hotspots tab** — cropped mini-maps for each region with an active disruption (Middle East, Russia, South America right now), instead of trying to read clusters off the single crowded global map.
2. **Changes tab** — a day/week/month log of what actually changed and when, with sources and confidence tags on every entry.
3. **Tenders tab** — who's buying crude right now, what they're switching from, and (where disclosed) volumes — e.g. Orlen's post-Petroline diversification and India's resumed Russian purchases.
4. **Clarified Cargo Substitution & Destinations tabs** — added a plain-language "In short" explainer to each, and cross-referenced them against the new Tenders tab so the difference between the three is clear.
5. **Corrected the Saudi East-West Pipeline (Petroline) status** — it was showing "elevated utilisation" (Amber) when it had actually been shut down 13 Sep 2026 by a drone attack. Now shows Red/shut down, with a new disruption entry and a HIGH-confidence flow figure.

There's also an honest answer built into the Data & Method tab on the AIS/tanker-tracking question: genuine free vessel-tracking data isn't achievable here (every route is either paid, requires your own receiver hardware, or isn't structured data) — so the Changes tab is the transparent substitute rather than a fabricated "AIS-derived" figure.

## What to do

1. In your `crude-flow-dashboard` repo on GitHub, click **Add file → Upload files**.
2. Drag in `index.html`, `data.json`, and `CLAUDE.md` from this bundle — they replace the versions already there. (`manifest.json`, `service-worker.js`, `icons/`, and the `.github/workflows/` folder are unchanged this round — you can upload them too, GitHub will just say there's nothing to change.)
3. Commit.

That's it — your daily automation will keep working exactly as before, now also maintaining the new `tenders` and `change_log` data as it runs.
