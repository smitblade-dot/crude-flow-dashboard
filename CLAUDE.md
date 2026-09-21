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

## ONE-TIME MIGRATION — do this first, before the normal fast check (added 2026-09-21)

`index.html` was updated 2026-09-21 to add a **Production layer**: every
`infrastructure` row needs a `layer` field, and there's a new Production
tab that only shows rows with `layer: "Production"`. `data.json` in this
repo does not have this yet. **Check first: if every row in
`infrastructure` already has a `layer` field, this migration has already
run — skip this whole section and go straight to the normal procedure
below.** Otherwise, in this same run, before anything else:

1. Read the current `data.json`.
2. For every existing row in `infrastructure`, add a `layer` field:
   `"Export"` if `type` is `"Export terminal"`, `"Pipeline"` if `type` is
   `"Pipeline"`. Do not change any other field on these rows.
3. Append these 39 new Production-layer rows to `infrastructure` (verbatim
   — these are already researched and written, don't regenerate or
   paraphrase them). For each one, set `infrastructure_id` to an
   incrementing integer starting one above the current highest
   `infrastructure_id` in the file, and set `region`, `latitude` and
   `longitude` by copying them from the matching `countries[]` row for
   that `country` (same values that row's existing `infrastructure`
   entries already use):

```json
[
  {
    "country": "Algeria",
    "infrastructure_name": "Hassi Messaoud / southern fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Hassi Messaoud / southern fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~1.0–1.4 mb/d crude + condensate (mature onshore, steady)",
    "status": "Normal — mature onshore base, steady output",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Angola",
    "infrastructure_name": "Deepwater Blocks 15/17/31 (Dalia, Kizomba, Girassol)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Deepwater Blocks 15/17/31 (Dalia, Kizomba, Girassol)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~1.0–1.1 mb/d (deepwater FPSOs, natural decline from 2010s peak)",
    "status": "Declining — mature deepwater fields past peak, new project sanctioning slow",
    "map_status": "Amber",
    "watch_item": "Long-run decline; watch new FPSO sanctioning (e.g. Agogo, Kaminho) for offset.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Cameroon",
    "infrastructure_name": "Rio del Rey / Ebome offshore fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Rio del Rey / Ebome offshore fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~55–60 kb/d (small, mature)",
    "status": "Normal — small, stable output",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Chad",
    "infrastructure_name": "Doba basin fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Doba basin fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~125–130 kb/d (Doba basin, exported via Chad–Cameroon pipeline)",
    "status": "Constrained — 2024–25 nationalisation dispute with operators still deterring new investment",
    "map_status": "Amber",
    "watch_item": "Nationalisation dispute has left upstream investment stalled; production flat rather than growing.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Congo (Republic)",
    "infrastructure_name": "Moho-Nord / Djeno deepwater fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Moho-Nord / Djeno deepwater fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~250–270 kb/d (deepwater, rising with new project start-ups)",
    "status": "Rising — new deepwater projects offsetting mature-field decline",
    "map_status": "Blue",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Côte d'Ivoire",
    "infrastructure_name": "Baleine deepwater field",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Baleine deepwater field",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~55–90 kb/d and rising sharply as Baleine Phase 2 ramps up",
    "status": "Rising fast — Eni's Baleine Phase 2 online, targeting further phases",
    "map_status": "Blue",
    "watch_item": "From near-zero pre-2023 to a fast-growing producer; capacity keeps stepping up with each Baleine phase.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Egypt",
    "infrastructure_name": "Western Desert / Gulf of Suez fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Western Desert / Gulf of Suez fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~600–615 kb/d (mature Western Desert & Gulf of Suez)",
    "status": "Normal — mature, broadly flat",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Equatorial Guinea",
    "infrastructure_name": "Zafiro / Ceiba offshore fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Zafiro / Ceiba offshore fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~80–85 kb/d (mature, declining)",
    "status": "Declining — ageing offshore fields, limited new drilling",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Gabon",
    "infrastructure_name": "Rabi-Kounga / deepwater blocks",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Rabi-Kounga / deepwater blocks",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~230–240 kb/d (broadly steady, some new deepwater)",
    "status": "Normal — steady with incremental deepwater additions",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Ghana",
    "infrastructure_name": "Jubilee / TEN offshore fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Jubilee / TEN offshore fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~120–190 kb/d (Jubilee/TEN, natural decline; new infill drilling ongoing)",
    "status": "Declining — natural field decline partly offset by infill wells",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Libya",
    "infrastructure_name": "Sharara / El Feel / Waha fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Sharara / El Feel / Waha fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~1.2–1.4 mb/d, but volatile — recurring blockades and force majeure declarations",
    "status": "Volatile — history of sudden shut-ins tied to political/security disputes",
    "map_status": "Amber",
    "watch_item": "Headline capacity is high but actual flow swings sharply with each blockade; treat any single figure as a snapshot.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Mauritania",
    "infrastructure_name": "Chinguetti field (depleted)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Chinguetti field (depleted)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "Effectively nil — Chinguetti ceased commercial output around 2017; no other producing crude field",
    "status": "Shut in — field depleted; country's current hydrocarbon story is the Greater Tortue Ahmeyim gas/LNG project, not crude",
    "map_status": "Amber",
    "watch_item": "Do not confuse with GTA — that is a gas/LNG project, tracked on the gas dashboard, not crude oil.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Nigeria",
    "infrastructure_name": "Bonny / Forcados / Qua Iboe + offshore FPSOs",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Bonny / Forcados / Qua Iboe + offshore FPSOs",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~1.4–1.7 mb/d, recovering as pipeline vandalism/crude theft crackdowns take hold",
    "status": "Recovering — output rebuilding off 2022 lows as onshore security improves",
    "map_status": "Blue",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Senegal",
    "infrastructure_name": "Sangomar field (FPSO, first oil 2024)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Sangomar field (FPSO, first oil 2024)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~95–100 kb/d, government assessing raising output above 100 kb/d",
    "status": "Rising — Senegal's first-ever producing field, still ramping",
    "map_status": "Blue",
    "watch_item": "Newest African producer in this dataset; 2026 output already a meaningful share of GDP.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "South Sudan",
    "infrastructure_name": "Unity / Upper Nile fields (Nile Blend)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Unity / Upper Nile fields (Nile Blend)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~150–180 kb/d — repeatedly interrupted by drone strikes on the Sudan transit pipeline, resumed after repairs",
    "status": "Fragile — production itself largely intact, but exports depend entirely on a pipeline that keeps getting hit",
    "map_status": "Amber",
    "watch_item": "The constraint is downstream (transit through Sudan), not the fields themselves — see the Export/Pipeline layer and Disruptions tab.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Sudan",
    "infrastructure_name": "Domestic Nile Blend fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Domestic Nile Blend fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~30 kb/d own production (small); Sudan's larger role is as transit host for South Sudan's export pipeline",
    "status": "Conflict-affected — own output minor and secondary to its transit role",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Tunisia",
    "infrastructure_name": "El Borma / Gulf of Gabes fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "El Borma / Gulf of Gabes fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~28–30 kb/d (small, mature)",
    "status": "Normal — small and stable",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Uganda",
    "infrastructure_name": "Kingfisher & Tilenga fields (Lake Albert basin)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Kingfisher & Tilenga fields (Lake Albert basin)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "Pre-production — Kingfisher/Tilenga targeting first oil around mid-2026; not yet flowing as of this snapshot",
    "status": "Pre-production — wells drilled and testing oil, but EACOP export pipeline and refinery still under construction",
    "map_status": "Blue",
    "watch_item": "Data gap: no confirmed first-oil date has been reached as of this snapshot — treat any production figure as a forward target, not an actual.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Bahrain",
    "infrastructure_name": "Awali onshore field + Abu Safah (shared with Saudi Arabia)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Awali onshore field + Abu Safah (shared with Saudi Arabia)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~190 kb/d (small, mature)",
    "status": "Normal — small, stable",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Iran",
    "infrastructure_name": "Ahvaz / Marun / Gachsaran fields (Khuzestan)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Ahvaz / Marun / Gachsaran fields (Khuzestan)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~3.3–4.0 mb/d, sanctions-constrained with periodic partial recovery",
    "status": "Constrained — sanctions cap upside; output flexes with enforcement intensity and buyer (mainly China) demand",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Iraq",
    "infrastructure_name": "Rumaila / West Qurna / Majnoon fields (Basra)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Rumaila / West Qurna / Majnoon fields (Basra)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~4.4–4.5 mb/d (OPEC+ quota-adjusted)",
    "status": "Normal — near quota, capacity headroom above it",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Israel",
    "infrastructure_name": "Meged field (onshore, marginal)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Meged field (onshore, marginal)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "Negligible — Israel is overwhelmingly a gas producer (Leviathan/Tamar); crude output is marginal",
    "status": "Marginal — not a meaningful crude producer",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Kuwait",
    "infrastructure_name": "Greater Burgan field",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Greater Burgan field",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~2.7–2.8 mb/d (OPEC+ quota-adjusted)",
    "status": "Normal — near quota",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Oman",
    "infrastructure_name": "PDO-operated onshore/offshore blocks",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "PDO-operated onshore/offshore blocks",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~1.0 mb/d (non-OPEC, steady)",
    "status": "Normal — steady, EOR-supported plateau",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Qatar",
    "infrastructure_name": "Dukhan + offshore fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Dukhan + offshore fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~0.6 mb/d crude (small relative to Qatar's condensate/NGL and gas output)",
    "status": "Normal — crude is a minor share of Qatar's liquids; most volume is condensate tied to gas production",
    "map_status": "Green",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Saudi Arabia",
    "infrastructure_name": "Ghawar / Safaniya fields (world's largest onshore & offshore fields)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Ghawar / Safaniya fields (world's largest onshore & offshore fields)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~9.5–11 mb/d and rising as OPEC+ voluntary cuts unwind through 2026, approaching spare capacity",
    "status": "Rising — cuts unwinding; Aramco maintained ~12 mb/d sustainable capacity through the cut period",
    "map_status": "Blue",
    "watch_item": "Upstream production is not currently the constrained link — see Export/Pipeline layer and Disruptions tab for the Red Sea/Bab al-Mandeb-related risk to loadings.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Syria",
    "infrastructure_name": "Northeast fields (Al-Omar, Deir ez-Zor basin)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Northeast fields (Al-Omar, Deir ez-Zor basin)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~80–90 kb/d and recovering as the new government consolidates control of previously SDF/factional-held fields",
    "status": "Recovering — post-Assad consolidation of field control under way through 2026; still far below pre-war ~380 kb/d",
    "map_status": "Blue",
    "watch_item": "Treat current-year figures as a recovering baseline, not a stable plateau — control of fields has changed hands multiple times.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "UAE",
    "infrastructure_name": "Upper Zakum / Murban fields (ADNOC)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Upper Zakum / Murban fields (ADNOC)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~4.0–4.3 mb/d, rising toward ADNOC's expanded ~5 mb/d capacity as OPEC+ cuts unwind",
    "status": "Rising — ADNOC capacity expansion largely complete; output tracking quota unwind",
    "map_status": "Blue",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Yemen",
    "infrastructure_name": "Marib / Shabwa fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Marib / Shabwa fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~15–50 kb/d — exports resumed mid-2026 after a prolonged Houthi-related blockade; still far below the pre-war ~400 kb/d",
    "status": "Resuming, but fragile — restart is recent and regional tension (Houthi/Red Sea) puts it at renewed risk",
    "map_status": "Amber",
    "watch_item": "Just came back online in 2026 after years shut in — a single disruption could reverse this quickly; watch alongside the Red Sea chokepoint situation.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Kazakhstan",
    "infrastructure_name": "Tengiz / Kashagan / Karachaganak fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Tengiz / Kashagan / Karachaganak fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~1.9–2.2 mb/d and rising on the completed Tengiz expansion, in recurring tension with OPEC+ quota compliance",
    "status": "Rising — Tengiz Future Growth Project output above quota has been a recurring OPEC+ friction point",
    "map_status": "Blue",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Russia",
    "infrastructure_name": "West Siberia / Volga-Urals / Sakhalin fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "West Siberia / Volga-Urals / Sakhalin fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~9.5–10.5 mb/d (OPEC+ quota-adjusted; sanctions have reshaped logistics, not underlying output)",
    "status": "Normal upstream — production broadly steady; the sanctions story is entirely in how/where the barrels move",
    "map_status": "Green",
    "watch_item": "Production itself is not the constraint — see the shadow-fleet/sanctions coverage in Disruptions and Destinations tabs.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Argentina",
    "infrastructure_name": "Vaca Muerta shale (Neuquén basin)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Vaca Muerta shale (Neuquén basin)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~0.85–0.98 mb/d and rising on Vaca Muerta shale growth",
    "status": "Rising — shale-driven growth, new export pipeline capacity (Oldelval/VMOS) supporting further increases",
    "map_status": "Blue",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Brazil",
    "infrastructure_name": "Pre-salt Santos/Campos basins (Búzios, Tupi/Lula)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Pre-salt Santos/Campos basins (Búzios, Tupi/Lula)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~4.3–4.7 mb/d and rising on pre-salt deepwater expansion",
    "status": "Rising — pre-salt FPSOs continue to ramp; Brazil now among the world's top producers",
    "map_status": "Blue",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Colombia",
    "infrastructure_name": "Llanos basin fields (Rubiales legacy, Castilla)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Llanos basin fields (Rubiales legacy, Castilla)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~0.76–0.78 mb/d (mature, gradual decline)",
    "status": "Declining — reserves replacement below depletion, exploration moratorium debate ongoing",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Ecuador",
    "infrastructure_name": "Amazon basin fields (Block 192/1AB), via SOTE/OCP pipelines",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Amazon basin fields (Block 192/1AB), via SOTE/OCP pipelines",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~0.44–0.47 mb/d, subject to periodic pipeline outages (spills, erosion, sabotage)",
    "status": "Prone to interruption — SOTE/OCP pipeline outages are a recurring, not one-off, risk",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Guyana",
    "infrastructure_name": "Stabroek block (Liza, Payara, Yellowtail)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Stabroek block (Liza, Payara, Yellowtail)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~0.90–0.92 mb/d and still ramping — Yellowtail reached full capacity in late 2025, Whiptail is next",
    "status": "Rising fast — from zero in 2019 to one of the fastest-growing producers globally; more FPSOs (Whiptail, Hammerhead) still to come",
    "map_status": "Blue",
    "watch_item": "The single fastest-growing production story in this dataset — recheck the figure each cycle rather than assuming a plateau.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Peru",
    "infrastructure_name": "Talara / Amazon basin (Block 192/1AB)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Talara / Amazon basin (Block 192/1AB)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~0.04–0.12 mb/d (small, Amazon output frequently shut in by pipeline spills/community blockades)",
    "status": "Volatile — NorPeru pipeline outages and Block 192 restart disputes repeatedly interrupt the Amazon share",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Suriname",
    "infrastructure_name": "Staatsolie onshore fields + GranMorgu (Block 58, pre-production)",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Staatsolie onshore fields + GranMorgu (Block 58, pre-production)",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~10–15 kb/d from existing onshore Staatsolie production; the major GranMorgu offshore project targets first oil around 2028 and is not yet flowing",
    "status": "Small today, transformative later — GranMorgu (TotalEnergies-operated) is under construction, not yet producing",
    "map_status": "Blue",
    "watch_item": "Do not confuse today's small onshore figure with GranMorgu's eventual ~220 kb/d design capacity — that is still years out.",
    "origin_lat": null,
    "origin_lon": null
  },
  {
    "country": "Venezuela",
    "infrastructure_name": "Orinoco Belt / Lake Maracaibo fields",
    "type": "Production field",
    "layer": "Production",
    "operator": null,
    "origin": "Orinoco Belt / Lake Maracaibo fields",
    "destination_terminal": "Domestic gathering / feeds export system",
    "destination_basin": "Domestic gathering / feeds export system",
    "mode": "Upstream production",
    "capacity": "~0.9–1.1 mb/d, slow recovery under partial US sanctions relief and Chevron-licensed output",
    "status": "Recovering — licensed operators (Chevron and others) have lifted output off post-sanctions lows, but well below historic capacity",
    "map_status": "Amber",
    "watch_item": null,
    "origin_lat": null,
    "origin_lon": null
  }
]
```

4. For each of the 39 countries above, set that country's row in
   `countries[]` — the `approximate_current_production` field (currently
   `null` on every row) — to the same text as that row's `capacity` value
   above.
5. Set `meta.generated` to the current timestamp as normal (step 5 below),
   commit with message `Data migration — add Production layer (2026-09-21)`,
   and push. Then continue with today's normal fast-check procedure below
   in the same run if you have turns to spare — but the migration commit
   alone, pushed on its own, is a complete and correct step if you don't.

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
- **Every `infrastructure` row must have a `layer` field** (`"Production"`,
  `"Export"`, or `"Pipeline"` — added 2026-09-21, see the migration section
  above). If you ever add a brand-new infrastructure row for a genuinely
  new asset, give it a `layer` too — an `Export`/`Pipeline` row with
  populated `origin_lat`/`origin_lon` draws an arc on the map; a
  `Production` row should have `origin_lat`/`origin_lon` set to `null` (it
  draws as a standalone node, not an arc) and a short generic
  `destination_terminal`/`destination_basin` like `"Domestic gathering /
  feeds export system"` rather than repeating the field name.

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
2. **Check the migration section above first** — if any `infrastructure`
   row is missing a `layer` field, do that migration now, in this run,
   before anything else.
3. Run the fast check (above). If it finds nothing material, skip to step
   6.
4. If the fast check found something real, do the deeper research it
   needs — free/public sources only: Reuters, Al Jazeera, Bloomberg,
   OilPrice.com, Argus, S&P Global, IEA, OPEC, EIA, national operator/NOC
   statements.
5. Edit `data.json` in place with the changes, following the schema and
   confidence rules above (including `tenders`/`change_log` where
   applicable). Keep the file valid JSON (check it parses).
6. Always set `meta.generated` to the current ISO 8601 UTC timestamp, then
   commit (message like `Data refresh — 2026-09-16T14:00Z` — or note what
   changed if something did) and push, using git directly:
   `git add data.json && git commit -m "..." && git push`. Do not touch
   any other file in this repo during a normal refresh.

Do not wait for approval or ask a question — this is a scheduled,
unattended run.
