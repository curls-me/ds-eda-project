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

## Slide 2 — Only 2 of 5 downtowns are actually pricier than the outskirts

- Pooling all 5 downtowns hid the effect (not significant, p = 0.46)
- Tested individually:
  - **Bellevue is 82.7% pricier** than outskirts (significant)
  - **Seattle is 23.9% pricier** (significant, but under the 50% bar)
  - Renton, Kent, Federal Way — cheaper, not significant

**[Table: median price by town]** — full height, right of text

| Town | Median price |
|---|---|
| Bellevue | $745,000 |
| Seattle | $569,950 |
| Other (outskirts) | $460,000 |
| Renton | $330,000 |
| Kent | $286,543 |
| Federal Way | $262,250 |

---

## Slide 3 — Seattle is the only downtown that's genuinely denser than the outskirts

- Downtown overall is 7.7% denser (significant)
- By city:
  - **Seattle's lots are 45.4% smaller** than outskirts (significant)
  - Kent, Renton, Federal Way — no significant difference
  - Bellevue — actually 26% *bigger*, not smaller

**[Chart: density_boxplot.png — neighboring lot size by town, smaller = denser]**

---

## Slide 4 — 11 homes match Nicole's criteria today — all in Seattle

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

## Slide 5 — Spring is when the market peaks — but the winter discount is real

- Sales aren't spread evenly across the year (p ≈ 0)
- Peak season: **Spring (30.2%)**, not Summer (29.3%) as assumed
- Winter homes sold for **5.5% less**
- Checked per sqft too: still **4.9% cheaper** in winter — the discount holds up

**[Chart: sales_by_season.png — sales volume by season]**
**[Chart: price_per_sqft_season.png — median $/sqft by season, winter highlighted]**

---

## Slide 6 — Three takeaways for Nicole

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
