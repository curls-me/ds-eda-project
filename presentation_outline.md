# Presentation Outline — King County Housing EDA for Nicole Johnson

Client: Nicole Johnson (buyer) — lively, central neighborhood, middle price range, timing within a year, 1-2 bedroom / 485-645 sqft.

10-minute talk → 9 content slides (cover + closing excluded from the count, per checklist budget of ~1 min/slide).

## Cover (not counted)
Title, your name, "Finding Nicole's home: a data-driven shortlist."

## S — Situation (1 slide)
1. **Nicole wants a lively, central home she can afford within a year**
   - Her criteria: 1-2 bedrooms, 485-645 sqft, near a downtown, middle price range, move within 12 months.

## C — Complication (2 slides)
2. **"Lively and central" isn't a column in the data — we had to define it**
   - Assumption: downtown = within 5 miles of Seattle, Bellevue, Kent, Renton, or Federal Way (rule of three note: state the 3 biggest assumptions here — downtown radius, no-kids/size range, timing/seasonality).
3. **Not every downtown actually earns the label**
   - Being 5 miles from a city center on a map doesn't guarantee it's priced or built like a real downtown — that has to be tested, not assumed.

## R — Resolution (5 slides)
4. **Downtown living costs more — but only in some cities**
   - Hypothesis 1 result: pooling all 5 downtowns hid the effect; Seattle shows a clear price premium over outskirts, others less so. (Fill in: which downtowns are statistically significant, median $ premium.)
5. **Some downtowns are genuinely denser, not just closer to a pin on a map**
   - Hypothesis 2 result: `sqft_lot15` as a density proxy — downtown lots are smaller/denser than outskirts, but again not evenly across all 5 cities. (Fill in: which downtowns pass.)
6. **Only [N] downtowns are both pricier and denser — Nicole's real shortlist**
   - The combined pass/fail table: a downtown only counts as "lively and central" if it clears both the price-premium test and the density test.
7. **Here's what she can actually afford there**
   - Mid-price band (25th-75th percentile of local 1-2BR sales) per qualifying downtown — her realistic budget range, not one number for "downtown" overall.
8. **The shortlist: homes matching all of Nicole's criteria**
   - Count of matching sales per qualifying downtown, at her size + price + location filters.

## Recommendations (1 slide, rule of three)
9. **Three takeaways for Nicole**
   - (1) Target [downtown(s) that passed both tests] first — they're the only ones that are both pricier and denser than outskirts.
   - (2) Budget [mid-price band per town] for a 1-2BR in her size range there.
   - (3) This is sold-home evidence (May 2014-May 2015), not live listings — use these zip codes/price bands as a filter on today's market, and revisit timing/seasonality before committing.

## Closing (not counted)
Caveats slide folded into takeaway #3 above rather than a separate slide, to stay in budget — mention verbally: `sqft_lot15` is a density proxy, not a direct "lively" measure (no walkability/nightlife data).

---

## Checklist cross-check
- [x] SCR structure applied (Situation: slide 1, Complication: slides 2-3, Resolution: slides 4-8, Recommendation: slide 9)
- [x] Sized to ~1 min/slide: 9 content slides for a 10-min talk
- [x] Rule of three: 3 assumptions (slide 2), 3 recommendations (slide 9)
- [x] Titles written as outcome claims, not topic labels (e.g. "Downtown living costs more — but only in some cities," not "Hypothesis 1 Results")
- [ ] Preattentive cues — apply when building actual slides (bold/color the one number that matters per slide)
- [ ] Visualization bar — each chart must support its slide's title; strip chartjunk (apply at slide-build stage)
- [x] Iterating at outline stage before touching slides

## Numbers still needed before slides 4-8 are final
The notebook has these hypothesis tests written but not yet executed with saved output (no numbers persisted in the notebook file). Before filling in the bracketed placeholders above, re-run `04_eda.ipynb` end-to-end (it loads from the cached CSV in `data/`, no DB needed) to get: per-town medians/p-values (H1), per-town `sqft_lot15` medians/p-values (H2), the combined pass/fail table, mid-price bands per qualifying town, and shortlist counts.
