# Updating your site with this round's changes (2026-09-16)

Your hourly automation is untouched by this update — don't change anything about the GitHub Action or the token. This is a pure UI/UX polish pass: same data schema, same `data.json`, just a nicer `index.html`. Only one file changes.

## What's new

1. **Hotspots tab is much lighter.** It was re-embedding the full world map outline once per region card; now every map (main + hotspots) references one shared shape, so the page is smaller and the tab opens faster.
2. **Zoom & pan on the main map.** Scroll/pinch to zoom, drag to pan, plus +/−/reset buttons in the map toolbar.
3. **Hover tooltips.** Hovering a route or marker shows a floating label instead of relying on the browser's slow native tooltip.
4. **Global search.** The box in the header searches routes, disruptions, tenders and substitution events at once and jumps you straight to the result.
5. **Shareable links.** Switching tabs (and opening a route's detail panel) now updates the URL, so you can copy/paste a link straight to a specific tab or route.
6. **CSV export.** The Routes, Disruptions and Tenders tabs each got an "Export CSV" button.
7. **Print / Save as PDF.** A printer icon in the header gives you a clean, chrome-free printout of whichever tab is open.
8. **"Biggest movers this week"** strip on the Overview tab, built from the week-over-week change already in your data.
9. **Recent history + a small trend line** in each route's detail panel, pulling from the Changes log — honestly empty until entries build up over time, not fabricated.
10. **Mobile: bottom tab bar** and bigger touch targets on phones, instead of a cramped top scroller.

## What to do

1. In your `crude-flow-dashboard` repo on GitHub, click **Add file → Upload files**.
2. Drag in `index.html` from this bundle — it replaces the version already there.
3. Commit.

That's it. `data.json`, `manifest.json`, `service-worker.js`, the icons and the GitHub Action are all unchanged this round — nothing else to touch, and your hourly refresh keeps running exactly as before.
