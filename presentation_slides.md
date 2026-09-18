# Finding Nicole's Home: A Data-Driven Shortlist

King County Housing EDA — presentation copy
(Edit this file directly; it's the source text for `presentation.html`.
Full talking points live in `speaking_notes.md`, kept separate so this stays just slide bullets.)

---

## Cover

**Finding Nicole's home: a data-driven shortlist**
Marina · King County housing data, May 2014 – May 2015

---

## Slide 1 — Situation, assumptions & hypotheses

**Meet Nicole — a single first-time buyer, no kids**

- 1–2 bedrooms, 485–645 sqft
- Wants lively and central, on a mid-range budget
- Buying within the next 12 months

**How we defined "lively and central"**
- Downtown = within 5 miles of Seattle, Bellevue, Kent, Renton, or Federal Way
- Timing and price both matter — factor in seasonality

**What we're testing**
- Downtown homes cost 50%+ more than the outskirts
- Smaller neighboring lots = denser = more "lively"
- Winter sales run up to 5% cheaper

**[Chart: map_downtowns.png — houses colored by closest downtown]** — full height, right of text

---

## Slide 2 — One year of sales, 70 zip codes — enough to compare neighbourhoods

- **21,597 sales** of **21,420 homes**, May 2014 – May 2015 (177 homes sold twice — kept, they're real re-sales, not duplicates)
- **70 zip codes** across King County, 50–602 sales each (median 283) — every area has enough sales to compare
- **21 columns** per sale: price, size, rooms, build quality, location, plus the 15 nearest neighbours
- The market it covers: **$78K–$7.7M** (median $450K), 370–13,540 sqft, built 1900–2015
- Gaps are small and fillable — `yr_renovated` 17.8%, `waterfront` 11.1%, `sqft_basement` 2.1%, `view` 0.3%: filled in, no rows thrown away (one 33-bedroom typo removed)
- **Not in the data: walkability, nightlife, commute time** — which is why "lively" needs a proxy

**[Table: what's available, grouped]** — full height, right of text

| Group | Columns |
|---|---|
| Price & date | price, date |
| Size | sqft_living, sqft_lot, sqft_above, sqft_basement |
| Rooms | bedrooms, bathrooms, floors |
| Build quality | grade, condition, view, waterfront, yr_built, yr_renovated |
| Location | zipcode, lat, long |
| Neighbours (15 nearest) | sqft_living15, sqft_lot15 |

---

## Slide 3 — Only 2 of 5 downtowns are actually pricier than the outskirts

- Pooling all 5 downtowns hid the effect (not significant, p = 0.46)
- Tested individually, each city vs. its own local outskirts:
  - **Bellevue is 31.9% pricier** than its own outskirts (significant, but under the 50% bar)
  - **Seattle is 29.1% pricier** (significant, but under the 50% bar)
  - Renton (-15.4%), Kent (-9.0%), Federal Way (-12.7%) — cheaper than their own outskirts, not significant

**[Table: median price, downtown vs. that city's own outskirts]** — full height, right of text

| Town | Downtown median | Outskirts median | % diff |
|---|---|---|---|
| Bellevue | $745,000 | $565,000 | +31.9% |
| Seattle | $569,950 | $441,375 | +29.1% |
| Renton | $330,000 | $390,000 | -15.4% |
| Kent | $286,543 | $314,950 | -9.0% |
| Federal Way | $262,250 | $300,250 | -12.7% |

---

## Slide 4 — Seattle is the only downtown that's genuinely denser than the outskirts

- Downtown overall is 7.7% denser (significant)
- By city:
  - **Seattle's lots are 45.4% smaller** than outskirts (significant)
  - Kent, Renton, Federal Way — no significant difference
  - Bellevue — actually 26% *bigger*, not smaller

**[Chart: density_boxplot.png — neighboring lot size by town, smaller = denser]**

---

## Slide 5 — 11 homes match Nicole's criteria today — all in Seattle

- Her size range (485–645 sqft, 1–2BR) in Seattle: **$280,500–$383,944** (middle 50% of the market)
- Capping at that price ceiling: **11 matching homes**, all in Seattle
- Ranked by build quality (grade, then condition)

**[Table: the 11-home shortlist, ranked by grade/condition]**

| Grade | Condition | Beds | Sqft | Price |
|---|---|---|---|---|
| 7 | 4 | 1 | 590 | $202,000 |
| 7 | 3 | 1 | 550 | $353,000 |
| 6 | 4 | 2 | 610 | $286,000 |
| 6 | 3 | 1 | 520 | $295,000 |
| 6 | 3 | 1 | 620 | $382,888 |
| 6 | 3 | 1 | 600 | $229,000 |
| 6 | 3 | 1 | 640 | $340,000 |
| 5 | 3 | 2 | 630 | $315,000 |
| 5 | 3 | 1 | 520 | $275,000 |
| 5 | 2 | 1 | 570 | $310,000 |
| 5 | 2 | 2 | 590 | $156,000 |

---

## Slide 6 — Spring is when the market peaks — but the winter discount is real

- Sales aren't spread evenly across the year (p ≈ 0)
- Peak season: **Spring (30.2%)**, not Summer (29.3%) as assumed
- Winter homes sold for **5.5% less**
- Checked per sqft too: still **4.9% cheaper** in winter — the discount holds up

**[Chart: sales_by_season.png — sales volume by season]**
**[Chart: price_per_sqft_season.png — median $/sqft by season, winter highlighted]**

---

## Slide 7 — Three takeaways for Nicole

1. Focus the search on **Seattle** — the only downtown that's genuinely lively (dense); budget roughly **$280K–$384K**
2. Start with the **11 matching homes**, best-graded and best-condition first
3. **Buy in winter** — the ~5% discount holds even after controlling for house size, worth roughly **$7K–$16K** on the shortlisted homes

**[Table: final shortlist, priced for winter]**

| Grade | Condition | Winter price | Savings vs. summer |
|---|---|---|---|
| 7 | 4 | $193,071 | $8,929 |
| 7 | 3 | $327,831 | $15,161 |
| 6 | 4 | $273,358 | $12,642 |
| 6 | 3 | $285,815 | $13,218 |
| 6 | 3 | $355,588 | $16,444 |
| 6 | 3 | $212,672 | $9,835 |
| 6 | 3 | $329,414 | $15,234 |
| 5 | 3 | $292,540 | $13,529 |
| 5 | 3 | $275,000 | $12,718 |
| 5 | 2 | $300,348 | $13,890 |
| 5 | 2 | $151,143 | $6,990 |

---

## Closing (say out loud, not a slide)

- This is sold-home data from May 2014–May 2015, not live listings — use these zip codes and price bands as a filter on today's market
- `sqft_lot15` is a density proxy, not a direct measure of "lively"
- Seasonality reflects one observed year, not a confirmed recurring pattern
