# Finding Nicole's Home: A Data-Driven Shortlist

King County Housing EDA — presentation copy
(Edit this file directly; it's the source text for `presentation.html`)

---

## Cover

**Finding Nicole's home: a data-driven shortlist**
Your name · King County housing data, May 2014 – May 2015

---

## Slide 1 — Situation

**Nicole wants a lively, central home she can afford within a year**

- 1–2 bedrooms, 485–645 sqft
- Near a downtown
- Middle price range
- Move within 12 months

---

## Slide 2 — Complication

**"Lively and central" isn't a column in the data — we had to define it**

- **Downtown** = within 5 miles of Seattle, Bellevue, Kent, Renton, or Federal Way
- **Her home** = 1–2 bedrooms, 485–645 sqft
- **Timing matters** = seasonality should factor into when she buys

---

## Slide 3 — Complication

**Not every downtown actually earns the label**

- Being 5 miles from a city center doesn't guarantee it's priced or built like a real downtown
- That has to be tested, not assumed

**[Chart: map_downtowns.png — houses colored by closest downtown]**

---

## Slide 4 — Resolution

**Only 2 of 5 downtowns are actually pricier than the outskirts**

- Pooling all 5 downtowns hid the effect (not significant, p = 0.46)
- Tested individually:
  - **Bellevue is 82.7% pricier** than outskirts (significant)
  - **Seattle is 23.9% pricier** (significant, but under the 50% bar)
  - Renton, Kent, Federal Way — cheaper, not significant

---

## Slide 5 — Resolution

**Seattle is the only downtown that's genuinely denser than the outskirts**

- Downtown overall is 7.7% denser (significant)
- By city:
  - **Seattle's lots are 45.4% smaller** than outskirts (significant)
  - Kent, Renton, Federal Way — no significant difference
  - Bellevue — actually 26% *bigger*, not smaller

---

## Slide 6 — Resolution

**Density, not price, decides "lively" — and Seattle is the only one that qualifies**

- A downtown only needs to be denser to count as "lively"
- Pricier is a bonus, not a requirement
- Of the 5 downtowns, only **Seattle** clears the density bar

---

## Slide 7 — Resolution

**11 homes match Nicole's criteria today — all in Seattle**

- Her size range (485–645 sqft, 1–2BR) in Seattle: **$280,500–$383,944** (middle 50% of the market)
- Capping at that price ceiling: **11 matching homes**, all in Seattle
- Ranked by build quality (grade, then condition)

---

## Slide 8 — Resolution

**Spring is when the market peaks — but the winter discount is real**

- Sales aren't spread evenly across the year (p ≈ 0)
- Peak season: **Spring (30.2%)**, not Summer (29.3%) as assumed
- Winter homes sold for **5.5% less**
- Checked per sqft too: still **4.9% cheaper** in winter — the discount holds up

**[Chart: sales_by_season.png — sales volume by season]**
**[Chart: price_per_sqft_season.png — median $/sqft by season, winter highlighted]**

---

## Slide 9 — Recommendation

**Three takeaways for Nicole**

1. Focus the search on **Seattle** — the only downtown that's genuinely lively (dense); budget roughly **$280K–$384K**
2. Start with the **11 matching homes**, best-graded and best-condition first
3. **Buy in winter** — the ~5% discount holds even after controlling for house size, worth roughly **$7K–$16K** on the shortlisted homes

---

## Closing (say out loud, not a slide)

- This is sold-home data from May 2014–May 2015, not live listings — use these zip codes and price bands as a filter on today's market
- `sqft_lot15` is a density proxy, not a direct measure of "lively"
- Seasonality reflects one observed year, not a confirmed recurring pattern
