# 2026 Strait of Hormuz Crisis — US Consumer Price Impact Model

> **Interactive, single-file HTML dashboard** projecting downstream US consumer price impacts across 17 goods and services categories as a function of Strait of Hormuz closure duration.

![Last Updated](https://img.shields.io/badge/last%20updated-Mar%2013%2C%202026-red)
![Crisis Day](https://img.shields.io/badge/crisis%20day-14-critical)
![Brent Crude](https://img.shields.io/badge/Brent-~%24101%2Fbbl-orange)
![Status](https://img.shields.io/badge/strait%20status-CLOSED-red)

---

## Background

On **February 28, 2026**, US and Israeli forces launched Operation Epic Fury — joint strikes on Iranian nuclear and military infrastructure. Iran retaliated by closing the Strait of Hormuz, the narrow waterway through which approximately **21 million barrels of oil per day** transit (~20% of global supply). Tanker traffic collapsed from 24 vessels/day to near zero within 72 hours.

This repository tracks the downstream economic consequences for US consumers through a duration-based price projection model, updated daily as the crisis evolves.

---

## What This Is

A **zero-dependency, single-file interactive HTML dashboard** that:

- Maps crisis duration (weeks 1–52) directly to consumer price pass-through percentages
- Models 17 goods and services categories with independent lag curves, anchor percentages, and theoretical maxima
- Updates all projections live as the user drags a duration slider
- Documents every formula, parameter, and source citation in a collapsible methodology panel
- Requires no server, no build step, no dependencies — open in any browser

This is a **scenario projection model**, not a financial forecast. It is built on empirical pass-through research from the 1973 embargo, 1979 Iranian revolution, and 2022 Ukraine war datasets.

---

## Features

- **Duration slider** — drag weeks 1–52, all 17 item prices recalculate instantly
- **Three-segment pass-through formula** — linear early ramp → power curve cascade → log-growth structural embedding
- **Per-item lag modeling** — gasoline moves within days; appliances and electronics take 5–6 weeks
- **Fixed anchor columns** — Week 4 (Point of No Return, ≈ Mar 28) and Week 13 (Full cascade, ≈ May 30) always visible as reference points; columns dim automatically when the slider passes them
- **Live status panel** — tanker traffic, oil prices, clock status updated with each daily revision
- **Three Doomsday Clocks** — refinery feedstock, commercial inventory, and forward contract freeze thresholds annotated on the slider
- **Snapshot cards** — gasoline, grocery basket, ground beef, and headline CPI impact at a glance
- **Methodology panel** — 6 sections covering model overview, milestone explainer, formula documentation, per-item parameter table, source citations, and known limitations
- **Mobile card layout** — responsive, works on phone

---

## The Model

### Pass-Through Formula

Each item runs through `passThrough(week, w4pct, w13pct, maxpct, lag)`:

| Segment | Range | Shape | Rationale |
|---|---|---|---|
| 1 — Early Ramp | `[0, w4]` | Linear | Empirical data is approximately linear before cascade effects activate |
| 2 — Cascade | `[w4, w13]` | Power curve (`t^0.75`) | Each upstream repricing triggers downstream repricing; concave acceleration |
| 3 — Structural | `[w13, 52]` | Log growth | Diminishing returns as demand destruction and substitution cap prices |

`effectiveWeek = week − lag` — each item only starts moving after its supply chain lag expires.

### Key Milestones

**Week 4 (≈ Mar 28) — Point of No Return**
Three thresholds converge simultaneously: refinery feedstock buffers exhaust (~16 days of crude on hand), commercial inventory falls below forward-contract minimums, and trade finance seizes as banks can't price LC risk. The cascade becomes self-reinforcing. A strait reopening after this point cannot prevent pass-through — it is already transmitted upstream into pricing assumptions and contract renegotiations.

**Week 13 (≈ May 30) — Full Pass-Through Complete**
The empirically documented horizon at which oil price shocks have fully propagated through all consumer price chain layers in past events. By week 13, every category has absorbed its first-round shock. After this, structural embedding begins — wage negotiations, long-term contracts, and rent resets incorporate the new price level, making inflation sticky regardless of what oil does next.

### Items Modeled

| Category | Items |
|---|---|
| Energy | Gasoline (regular), Diesel / heating oil, Electricity (kWh) |
| Food | Eggs, Milk, Bread, Chicken, Ground beef, Produce, Coffee |
| Services | Airfare (round-trip domestic), Restaurant meal |
| Manufactured goods | Appliances (mid-range), New vehicle, Clothing, Laptop / electronics |

### The $3/Day Rule

Some analysts predicted WTI would rise ~$3/day while the strait remains closed. **Actual tracking as of Day 14: ~$2.38/day** ($66 → $97 over 13 days). The rule is a valid early-crisis heuristic but breaks down beyond the first few weeks — linear extrapolation to week 13 (91 days) implies WTI ~$340, which is physically impossible. Demand destruction, SPR releases, and recession-driven consumption collapse create natural price ceilings. This model correctly captures the ceiling by mapping directly to empirical pass-through percentages rather than a linear oil price ramp.

---

## Current Status (Day 14 — Mar 13, 2026)

| Indicator | Status |
|---|---|
| Tanker transits | 1–2/day (was 24/day normal) |
| WTI | ~$97/bbl |
| Brent | ~$101/bbl (peak ~$120 on Mar 9) |
| Navy escorts | Suspended — strait designated "kill box" |
| Last bypass route | Oman Mina Al Fahal terminal evacuated |
| Gulf production cut | ≥10 mb/d (IEA) — losses set to increase |
| Clock 1 (refinery feedstock) | **EXPIRES TODAY / TOMORROW (Mar 14–16)** |
| Clock 2 & 3 (commercial inventory + LC freeze) | Mar 18–21 |
| Political signal | Trump: "I have plenty of time" — long-war posture |

### Analyst Price Forecasts

| Source | Scenario | Brent Target |
|---|---|---|
| Goldman Sachs | Base case (Mar–Apr) | $98/bbl |
| Goldman Sachs | Extreme (30-day disruption) | $110/bbl |
| KPMG (Diane Swonk) | 6-month conflict | $130+/bbl |
| Wood Mackenzie | Near-term | $150/bbl |
| Oxford Economics | "Breaking point" scenario | $140/bbl avg over 2 months |
| Iran IRGC (threat) | Open-ended | $200/bbl |

---

## File Structure

```
hormuz-price-model/
├── README.md                                  ← this file
└── hormuz_price_projections_interactive.html  ← the dashboard (single file, open in browser)
```

No `package.json`. No build step. No CDN calls. One file.

---

## Updating the Dashboard

The dashboard is updated daily. Each revision touches:

1. **Datestamp and day count** in the executive summary header
2. **Live status panel** — tanker traffic, oil price, new escalations
3. **Key facts strip** — clock countdowns, WTI move, production cut figures
4. **Warning strip** — updated analyst consensus
5. **Item parameters** (`data-w4pct`, `data-w13pct`, `data-maxpct` attributes) — adjusted when analyst forecasts materially shift
6. **Methodology panel** — any new source citations or limitations

All item parameters are stored as HTML `data-*` attributes on each `.price-row` element and consumed by the JS pricing engine at runtime — no recompilation required.

---

## Data Sources

| Source | Used For |
|---|---|
| BLS CPI (Jan 2026) | All baseline consumer prices |
| Federal Reserve DSGE model | Oil-to-CPI pass-through estimates |
| UCLA Anderson Supply Chain (2023) | Indirect effect calibration, power curve exponent (0.75) |
| IMF Working Paper WP/22/173 | Cross-country oil shock pass-through |
| ScienceDirect US weekly pass-through | Weekly granularity gasoline anchors |
| 1973 Embargo / 1979 Revolution analogs | Segment 3 absoluteMax multiplier (×1.5), log curve shape |
| Goldman Sachs (Mar 2026) | WTI/Brent price trajectory cross-validation |
| IEA Oil Market Report (Mar 2026) | Supply disruption magnitude, production cut figures |

---

## Known Limitations

- **No live oil price feed** — duration maps to pass-through directly; a week-8 crisis at $85/bbl differs from one at $115/bbl
- **US-centric** — pass-through rates calibrated for US market only
- **No demand destruction modeled** — projections at week 26+ may be upper bounds
- **No policy intervention modeled** — SPR releases, price controls, or rationing not assumed
- **Concurrent financial stress not modeled** — equity declines, credit tightening, and bank stress feed into prices through separate channels
- **Segment 3 is extrapolated** — no modern 52-week Hormuz closure exists in the empirical record

---

## Disclaimer

This is a scenario projection model for analytical and educational purposes. It is **not a financial forecast**. Projections are built on empirical research from historical oil shocks and do not constitute investment, financial, or economic advice. For personal financial decisions, consult a licensed advisor.

---

*Maintained by Brooks — updated daily while the strait remains closed.*