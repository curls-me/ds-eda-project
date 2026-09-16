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
   - Three assumptions stated up front (rule of three): (1) downtown = within 5 miles straight-line of Seattle, Bellevue, Kent, Renton, or Federal Way; (2) her home is 1-2 bedrooms, 485-645 sqft; (3) timing and price matter, but seasonality should factor into when she buys.
3. **Not every downtown actually earns the label**
   - Being 5 miles from a city center on a map doesn't guarantee it's priced or built like a real downtown — that has to be tested, not assumed.

## R — Resolution (5 slides)
4. **Only 2 of 5 downtowns are actually pricier than the outskirts**
   - Pooling all 5 downtowns together hid the effect (not significant, p=0.46). Tested individually: **Bellevue is 82.7% pricier** than outskirts (significant, clears the "50%+" hypothesis) and **Seattle is 23.9% pricier** (significant, but doesn't clear the 50% bar). Renton, Kent, and Federal Way are actually *cheaper* than outskirts and not statistically significant.
5. **Seattle is the only downtown that's genuinely denser than the outskirts**
   - Downtown overall is 7.7% denser than outskirts on `sqft_lot15` (significant). But by city: **Seattle's lots are 45.4% smaller** than outskirts (significant) — Kent, Renton, and Federal Way show no significant difference, and Bellevue's lots are actually *26% bigger* than outskirts, not smaller.
6. **Seattle is the only downtown that clears both bars**
   - Combined test: a downtown only counts as "lively and central" if it's both significantly pricier *and* significantly denser than outskirts. Bellevue passes on price but fails on density. Renton, Kent, and Federal Way fail both. **Only Seattle passes both.**
7. **A realistic Seattle budget: $362K–$551K for a 1-2BR — and 5 homes match today**
   - Seattle's mid-price band (25th–75th percentile of local 1-2BR sales) is $361,821–$550,700 (n=851). Applying all of Nicole's criteria (1-2BR, 485-645 sqft, in-band price) finds **9 matching homes total**: 5 in Seattle, 3 in Renton, 1 in Kent. Flag Renton/Kent as cheaper fallbacks, not primary matches — they didn't clear the density bar in slide 5.
8. **Spring, not summer, is when the market actually peaks**
   - Sales aren't evenly spread across the year (chi-square, p≈0) — but the peak is **Spring (30.2%)**, not the assumed Summer (29.3%, a close second). Winter homes sold for 5.5% less than the rest of the year on raw numbers (significant) — but controlling for house size/quality/location, that "winter discount" nearly disappears (+0.5%, not significant). It's mostly which houses sell in winter, not a real seasonal dip. (Caveat: one 13-month window, not multiple years.)

## Recommendations (1 slide, rule of three)
9. **Three takeaways for Nicole**
   - (1) Focus the search on **Seattle** — the only downtown that's both statistically pricier and denser than outskirts; budget roughly $362K–$551K for a 1-2BR.
   - (2) Of the 9 homes matching her exact criteria today, 5 are in Seattle — start there; Renton/Kent options are cheaper but unconfirmed on "lively."
   - (3) Don't wait for a winter discount — sales peak in spring, and the apparent winter price dip mostly reflects which houses sell then, not a real seasonal deal.

## Closing (not counted)
Caveats to mention verbally rather than as a slide (stay in budget): this is sold-home data from May 2014–May 2015, not live listings — use these zip codes/price bands as a filter on today's market; `sqft_lot15` is a density proxy, not a direct "lively" measure; seasonality reflects one observed year, not a confirmed recurring pattern.

---

## Checklist cross-check
- [x] SCR structure applied (Situation: slide 1, Complication: slides 2-3, Resolution: slides 4-8, Recommendation: slide 9)
- [x] Sized to ~1 min/slide: 9 content slides for a 10-min talk
- [x] Rule of three: 3 assumptions (slide 2), 3 recommendations (slide 9)
- [x] Titles written as outcome claims, not topic labels (e.g. "Only 2 of 5 downtowns are actually pricier than the outskirts," not "Hypothesis 1 Results")
- [ ] Preattentive cues — apply when building actual slides (bold/color the one number that matters per slide, e.g. "Seattle" and "5 homes")
- [ ] Visualization bar — each chart must support its slide's title; strip chartjunk (apply at slide-build stage)
- [x] Iterating at outline stage before touching slides
