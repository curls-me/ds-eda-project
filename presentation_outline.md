# Presentation Outline — King County Housing EDA for Nicole Johnson

Client: Nicole Johnson (buyer) — lively, central neighborhood, middle price range, timing within a year, 1-2 bedroom / 485-645 sqft.

10-minute talk → 10 content slides (cover + closing excluded from the count, per checklist budget of ~1 min/slide).

## Cover (not counted)
Title, your name, "Finding Nicole's home: a data-driven shortlist."

## S — Situation (2 slides)
1. **Nicole wants a lively, central home she can afford within a year**
   - Her criteria: 1-2 bedrooms, 485-645 sqft, near a downtown, middle price range, move within 12 months.

2. **One year of sales, 70 zip codes — enough to compare neighbourhoods**
   - 21,597 sales of 21,420 homes (May 2014–May 2015; 177 homes sold twice, kept as real re-sales), across 70 King County zip codes with 50–602 sales each. 21 columns: price/date, size, rooms, build quality, location, and the 15-nearest-neighbour columns. Prices $78K–$7.7M (median $450K), 370–13,540 sqft, built 1900–2015. Missing data is minor and imputed rather than dropped. Nothing in the data measures walkability, nightlife, or commute — which is what forces the proxy on the next slide.

## C — Complication (2 slides)
3. **"Lively and central" isn't a column in the data — we had to define it**
   - Three assumptions stated up front (rule of three): (1) downtown = within 5 miles straight-line of Seattle, Bellevue, Kent, Renton, or Federal Way; (2) her home is 1-2 bedrooms, 485-645 sqft; (3) timing and price matter, but seasonality should factor into when she buys.
4. **Not every downtown actually earns the label**
   - Being 5 miles from a city center on a map doesn't guarantee it's priced or built like a real downtown — that has to be tested, not assumed.

## R — Resolution (5 slides)
5. **Only 2 of 5 downtowns are actually pricier than the outskirts**
   - Pooling all 5 downtowns together hid the effect (not significant, p=0.46). Tested individually, each city against its own local outskirts: **Bellevue is 31.9% pricier** than its own outskirts (significant, but doesn't clear the "50%+" hypothesis) and **Seattle is 29.1% pricier** (also significant, also under the 50% bar). Renton (-15.4%), Kent (-9.0%), and Federal Way (-12.7%) are actually *cheaper* than their own outskirts and not statistically significant.
6. **Seattle is the only downtown that's genuinely denser than the outskirts**
   - Downtown overall is 7.7% denser than outskirts on `sqft_lot15` (significant). But by city: **Seattle's lots are 45.4% smaller** than outskirts (significant) — Kent, Renton, and Federal Way show no significant difference, and Bellevue's lots are actually *26% bigger* than outskirts, not smaller.
7. **Density, not price, is what actually decides "lively" — and Seattle is the only downtown that qualifies**
   - A downtown only needs to be genuinely denser than the outskirts to count as "lively" — being pricier too is a bonus, not a requirement, since a lively-but-cheaper downtown is a *good* outcome for Nicole. Of the 5, only **Seattle** clears the density bar (slide 5); Bellevue is pricier but not denser, so it doesn't qualify on this criterion.
8. **11 homes match Nicole's criteria today — all in Seattle**
   - Filtering directly on her target size (485-645 sqft, 1-2BR) within Seattle, the middle of the market runs $280,500–$383,944 (25th-75th percentile). Capping at that ceiling (cheaper is fine too) finds **11 matching homes**, all in Seattle — the only downtown that passed the density test. Ranked by build quality (`grade`, then `condition` as tiebreaker) so the best-built options lead the list.
9. **Spring is when the market peaks — but the winter discount is real**
   - Sales aren't evenly spread across the year (chi-square, p≈0) — the peak is **Spring (30.2%)**, not the assumed Summer (29.3%, a close second). Winter homes sold for **5.5% less** than the rest of the year (significant). Checked against house size directly (price per square foot, not a full regression): winter is still **4.9% cheaper per sqft** (significant) — the discount holds up, it isn't just an artifact of which houses happen to sell in winter.

## Recommendations (1 slide, rule of three)
10. **Three takeaways for Nicole**
   - (1) Focus the search on **Seattle** — the only downtown that's genuinely denser (and lively) than its outskirts; realistic budget for her size range is roughly $280K–$384K.
   - (2) Start with the **11 matching homes**, led by the highest-graded, best-condition ones — every one already meets her size, location, density, and budget criteria.
   - (3) **Buy in winter** — the ~5% seasonal discount holds up even after controlling for house size, worth roughly $7K-$16K on the shortlisted homes specifically.

## Closing (not counted)
Caveats to mention verbally rather than as a slide (stay in budget): this is sold-home data from May 2014–May 2015, not live listings — use these zip codes/price bands as a filter on today's market, and note we're assuming that pattern still holds and that a shortlisted home's price stands in for a comparable listing today; `sqft_lot15` is a density proxy, not a direct "lively" measure; seasonality reflects one observed year, not a confirmed recurring pattern.

---

## Checklist cross-check
- [x] SCR structure applied (Situation: slides 1-2, Complication: slides 3-4, Resolution: slides 5-9, Recommendation: slide 10)
- [x] Sized to ~1 min/slide: 10 content slides for a 10-min talk
- [x] Rule of three: 3 assumptions (slide 3), 3 recommendations (slide 10)
- [x] Titles written as outcome claims, not topic labels (e.g. "Only 2 of 5 downtowns are actually pricier than the outskirts," not "Hypothesis 1 Results")
- [ ] Preattentive cues — apply when building actual slides (bold/color the one number that matters per slide, e.g. "Seattle," "11 homes," "winter")
- [ ] Visualization bar — each chart must support its slide's title; strip chartjunk (apply at slide-build stage)
- [x] Iterating at outline stage before touching slides
