# Speaking Notes — Finding Nicole's Home

Fuller talking points for each slide in `presentation.html` / `presentation_slides.md`.
Slides stay bullet-only; say the rest out loud.

---

## Cover

Introduce yourself and the client: Nicole Johnson, a buyer we're helping shortlist homes for using King County housing sale data (May 2014 – May 2015).

## Slide 1 — Situation, assumptions & hypotheses

- Nicole is a single, first-time buyer with no children, looking for a 1–2 bedroom home in the 485–645 sqft range (45–60 sqm).
- "Lively and central" isn't a column in the dataset — we had to define it ourselves, and we're stating the definition up front rather than assuming it:
  - **Downtown** = within 5 miles straight-line distance of one of the 5 biggest King County cities (Seattle, Bellevue, Kent, Renton, Federal Way). Straight-line, not driving distance — a simplification, not actual commute time. The 5-mile radius is our own assumption, not derived from the data.
  - Timing and price matter to her, but she should let seasonality factor into when she buys.
  - We're assuming the seasonal pattern found in this 2014–2015 dataset still holds today, and that the shortlisted homes stand in for current listings — their recorded price reflects roughly what a comparable home would cost now, adjusted for whichever season she buys in.
- Three hypotheses we're about to test, stated as a "rule of three" so it's clear we're not cherry-picking after the fact:
  1. Downtown homes have a median price at least 50% higher than homes outside that radius.
  2. Neighborhoods where nearby homes sit on smaller lots (lower `sqft_lot15`) are denser and more likely to be the lively, urban areas Nicole wants.
  3. Summer sales volume peaks, and winter homes sell for up to 5% less.
- Show the map here to sanity-check the downtown tagging itself before trusting any test built on it — the 5-mile radius should visibly cluster around each city center, surrounded by "Other."

## Slide 2 — Only 2 of 5 downtowns are actually pricier than the outskirts

- Pooling all 5 downtowns together hid the effect entirely (not significant, p = 0.46) — this is the reason we broke it out by city instead of testing "downtown" as one blob.
- Tested each city against its own local outskirts (not one shared outskirts number) — the area just outside Bellevue isn't priced like the area just outside Federal Way, so pooling them would have been misleading.
- Bellevue is 31.9% pricier than its own outskirts and significant, but falls short of the 50%+ hypothesis. Seattle is pricier too (29.1%) and significant, also under the 50% bar.
- Renton, Kent, and Federal Way are actually cheaper than their own outskirts, and not statistically significant — so "downtown = pricier" only holds for 2 of the 5 cities, and even those 2 don't clear the 50% bar we set going in.
- The median-price table on this slide is the same ordering used throughout: highest to lowest, now with each city's own outskirts median and % diff alongside it.

## Slide 3 — Seattle is the only downtown that's genuinely denser than the outskirts

- This tests Hypothesis 2: `sqft_lot15` (average lot size of a house's 15 nearest neighbors) as a proxy for density/urbanicity — the dataset has no direct measure of walkability or foot traffic, so smaller neighboring lots is a reasonable stand-in, but it's a proxy, not a fact.
- Downtown overall is significantly denser than outskirts (7.7%), but that pooled number hides a lot of variation by city, same pattern as the price test.
- Seattle's lots are 45.4% smaller than outskirts and significant — a real density signal, not just closeness to a map point.
- Bellevue is notable here: it passed the *price* test on the previous slide but its lots are actually 26% bigger than outskirts, not smaller — so despite being pricier, Bellevue is not denser/livelier by this measure.

## Slide 4 — 11 homes match Nicole's criteria today — all in Seattle

- We changed our filtering logic here: instead of requiring a downtown to pass both the price test AND the density test, we only require it to pass the density test (Hypothesis 2). A dense downtown that happens to be cheaper is a good outcome for Nicole, not a strike against it — her priority is "lively," not "priciest."
- Only Seattle passes the density significance test among the 5 downtowns, which is why every qualifying home is in Seattle.
- Price ceiling: the 75th percentile of prices for 1–2 bed, 485–645 sqft homes across the towns that passed the density test (just Seattle here) — anything cheaper is fine too, we're only capping the top end.
- The table is sorted by `grade` (construction/design quality) first, `condition` (upkeep) as a tiebreaker, so the best-built homes lead the list — this is the same ordering carried through to the final recommendation on slide 6.

## Slide 5 — Spring is when the market peaks — but the winter discount is real

- Two separate claims tested here: sales volume seasonality, and a winter price discount.
- Chi-square test on the season split confirms sales are not spread evenly through the year (p ≈ 0) — but the actual peak is Spring (30.2%), not Summer as originally hypothesized (Summer is a close second at 29.3%).
- Winter price discount: raw median price is 5.5% lower in winter (Mann-Whitney, significant). Because a raw price gap could just mean different *kinds* of houses sell in different seasons, we checked price-per-square-foot too (removes house size as a confound) — still 4.9% cheaper in winter, and still significant. The discount isn't an artifact of which houses happen to sell when.
- We haven't covered regression yet in the bootcamp, so this per-sqft comparison is the deliberately simpler way to control for size.

## Slide 6 — Three takeaways for Nicole

- This builds directly on the Slide 4 shortlist (11 Seattle homes, sorted by grade/condition) — nothing new is being filtered here, we're just adding a price-by-season projection on top.
- Each home's price is projected across all four seasons: we back out the effect of the season it actually sold in, then reapply each season's own effect, so we get an apples-to-apples "what would this same home cost in winter vs. spring vs. summer vs. fall" estimate.
- Every one of the 11 homes comes out cheapest in winter — the discount isn't cherry-picked, it holds across the whole shortlist. Savings vs. summer range roughly $7K–$16K depending on the home.
- Bottom line to leave Nicole with: shop the Seattle shortlist, favor the higher-graded homes at the top of the list, and time the purchase for winter to capture the seasonal discount on top of everything else.

## Closing (say out loud, not a slide)

- This is sold-home data from May 2014–May 2015, not live listings. Use the zip codes and price bands identified here as a filter on today's market — the underlying prices will have moved on.
- We're assuming the seasonal pattern documented here (winter discount, spring peak) still holds today; it's based on one observed year, not a confirmed recurring pattern.
- `sqft_lot15` is a density proxy, not a direct measure of "lively" — things like walkability and nightlife aren't in this dataset at all.
