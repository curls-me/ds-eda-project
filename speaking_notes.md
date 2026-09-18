# Speaking Notes — Finding Nicole's Home

Fuller talking points for each slide in `presentation.html` / `presentation_slides.md`.
Slides stay bullet-only; say the rest out loud.

---

## Cover

Introduce yourself and the client: Nicole Johnson, a buyer we're helping shortlist homes for using King County housing sale data (May 2014 – May 2015).

## Slide 1 — Situation, assumptions & hypotheses

- Nicole is a single, first-time buyer with no children, looking for a 1–2 bedroom home in the 485–645 sqft range (45–60 sqm).
- "Lively and central" isn't a column in the dataset — we had to define it ourselves, and we're stating both halves of the definition up front rather than assuming them:
  - **Central** = within 5 miles straight-line distance of one of the 5 biggest King County cities (Seattle, Bellevue, Kent, Renton, Federal Way). Straight-line, not driving distance — a simplification, not actual commute time. The 5-mile radius is our own assumption, not derived from the data.
  - **Lively** = dense, measured by `sqft_lot15`, the average lot size of a home's 15 nearest neighbours. Smaller neighbouring lots means homes packed closer together. Flag it as a proxy here and expand on it when it does the real work on slide 4.
- Two assumptions to say out loud here rather than put on the slide: timing and price both matter to Nicole, so seasonality should shape *when* she buys; and we're assuming the seasonal pattern in this 2014–2015 data still holds today, with the shortlisted homes standing in for comparable current listings.
- Three things we want to confirm or reject, stated as a "rule of three" so it's clear we're not cherry-picking after the fact:
  1. Downtown sale prices are at least 50% higher than homes outside that radius.
  2. Downtowns are denser than their outskirts — smaller `sqft_lot15`, the urban areas Nicole wants.
  3. Winter sale prices are lower, by up to 5%.
- Say plainly that these are stated before the results, and that two of the three come back partly or wholly rejected — that's the point of writing them down first.
- Show the map here to sanity-check the downtown tagging itself before trusting any test built on it — the 5-mile radius should visibly cluster around each city center, surrounded by "Other."

## Slide 2 — One year of sales, 70 zip codes

- Orient the audience before any findings: what the dataset actually is, so every later number has a known base.
- 21,597 recorded sales covering 21,420 distinct homes over roughly one year (May 2014 – May 2015). The 177 extra rows are homes that sold twice inside the window — we kept them deliberately: they're genuine re-sales at different prices, not duplicate records.
- 70 zip codes, with 50 to 602 sales each (median 283). Worth saying out loud: even the thinnest zip code has ~50 sales, so the neighbourhood-level comparisons later aren't built on a handful of rows.
- 21 columns per sale, grouped on the slide rather than listed one by one — price and date, size, room counts, build quality, location, and the `*15` columns that describe a home's 15 nearest neighbours (which is what the density test on slide 4 leans on).
- Range check: prices span $78K to $7.7M with a median of $450K, homes run 370 to 13,540 sqft, build years 1900 to 2015. Nicole's 485–645 sqft target sits at the very bottom of that size range — flag that early, it explains why the shortlist is small.
- Missing data is minor and each gap had a defensible fill (basement derived from living minus above-ground, view/waterfront default to 0, renovation year to 0 with a `was_renovated` flag kept). We imputed rather than dropped rows, so we didn't throw away good data in 20 other columns. The only row actually removed was a 33-bedroom / 1,620 sqft entry — a data-entry error, not a house.
- Close on the limitation, because it sets up the next slide: there is no walkability, nightlife, or commute column anywhere in this data. "Lively" is not measurable directly here, so it has to be proxied — that's the whole reason for the density test that follows.

## Slide 3 — Only 2 of 5 downtowns are actually pricier than the outskirts

- Take the assumption box first, before any result. Every home has a lat/long, so we hard-coded the coordinates of the 5 city centres and computed the haversine (great-circle) distance from each home to each centre. The nearest of those five decides which city a home belongs to; if even the nearest is more than 5 miles away, the home is outskirts. Same calculation runs over the whole cleaned dataset, so the tagging is reusable for later slices.
- Two caveats to say out loud: it's straight-line distance, not drive time, so a home 4 miles away across water is treated as closer than one 6 miles away down a straight road. And the 5-mile radius is our own pick — nothing in the data suggests it. Homes near the boundary would flip groups under a different radius.
- The map on slide 1 is the sanity check for exactly this: the tagging should show up as visible clusters around each city centre, surrounded by "Other". If someone doubts the definition, point back at that map.
- Pooling all 5 downtowns together hid the effect entirely (not significant, p = 0.46) — this is the reason we broke it out by city instead of testing "downtown" as one blob.
- Tested each city against its own local outskirts (not one shared outskirts number) — the area just outside Bellevue isn't priced like the area just outside Federal Way, so pooling them would have been misleading.
- Bellevue is 31.9% pricier than its own outskirts and significant, but falls short of the 50%+ hypothesis. Seattle is pricier too (29.1%) and significant, also under the 50% bar.
- Renton, Kent, and Federal Way are actually cheaper than their own outskirts, and not statistically significant — so "downtown = pricier" only holds for 2 of the 5 cities, and even those 2 don't clear the 50% bar we set going in.
- The median-price table on this slide is the same ordering used throughout: highest to lowest, now with each city's own outskirts median and % diff alongside it.

## Slide 4 — Seattle is the only downtown that's genuinely denser than the outskirts

- Start with the assumption box, out loud, before any result: we are assuming that smaller average lot size means higher density, and that higher density means "livelier." Say it as an assumption, not a finding — it's the load-bearing judgement call on this slide and the audience should get to disagree with it up front.
- The mechanics: `sqft_lot15` is the average lot size of a house's 15 nearest neighbours. Small neighbouring lots means homes packed close together; large ones mean spread-out, suburban plots. That's the density read.
- The second leap is density → lively. The dataset has no walkability score, no foot traffic, no nightlife or transit data, so there is nothing to measure "lively" with directly. Density is the closest available stand-in. Worth naming that this is two proxies stacked, and that a dense area could still be dull.
- If challenged: the honest answer is that the alternative was to drop the "lively" criterion entirely, and a defensible proxy stated openly beats a silent one.
- Downtown overall is significantly denser than outskirts (7.7%), but that pooled number hides a lot of variation by city, same pattern as the price test.
- Seattle's lots are 45.4% smaller than outskirts and significant — a real density signal, not just closeness to a map point.
- Bellevue is notable here: it passed the *price* test on the previous slide but its lots are actually 26% bigger than outskirts, not smaller — so despite being pricier, Bellevue is not denser/livelier by this measure.

## Slide 5 — 11 homes match Nicole's criteria today — all in Seattle

- We changed our filtering logic here: instead of requiring a downtown to pass both the price test AND the density test, we only require it to pass the density test (Hypothesis 2). A dense downtown that happens to be cheaper is a good outcome for Nicole, not a strike against it — her priority is "lively," not "priciest."
- Only Seattle passes the density significance test among the 5 downtowns, which is why every qualifying home is in Seattle.
- Price ceiling: the 75th percentile of prices for 1–2 bed, 485–645 sqft homes across the towns that passed the density test (just Seattle here) — anything cheaper is fine too, we're only capping the top end.
- The table is sorted by `grade` (construction/design quality) first, `condition` (upkeep) as a tiebreaker, so the best-built homes lead the list — this is the same ordering carried through to the final recommendation on slide 7.

## Slide 6 — Spring is when the market peaks — but the winter discount is real

- Two separate claims tested here: sales volume seasonality, and a winter price discount.
- Chi-square test on the season split confirms sales are not spread evenly through the year (p ≈ 0) — but the actual peak is Spring (30.2%), not Summer as originally hypothesized (Summer is a close second at 29.3%).
- Winter price discount: raw median price is 5.5% lower in winter (Mann-Whitney, significant). Because a raw price gap could just mean different *kinds* of houses sell in different seasons, we checked price-per-square-foot too (removes house size as a confound) — still 4.9% cheaper in winter, and still significant. The discount isn't an artifact of which houses happen to sell when.
- Be precise about what 4.9% compares: winter against *the rest of the year pooled*. Slide 7 quotes 7.1% instead, because there it's winter against spring alone, and spring is the priciest season per sqft. Same data, narrower comparison — expect this question.
- We haven't covered regression yet in the bootcamp, so this per-sqft comparison is the deliberately simpler way to control for size.

## Slide 7 — Three takeaways for Nicole

- This builds directly on the Slide 5 shortlist (11 Seattle homes, sorted by grade/condition) — nothing new is being filtered here, we're just adding a price-by-season projection on top.
- Each home's price is projected across all four seasons. The slide now shows the mechanics, so walk it slowly:
  1. Take the median price per square foot in each season and divide it by the all-year median $/sqft. That gives each season an index against an average year: Winter 0.959, Spring 1.033, Summer 1.004, Fall 0.990. Below 1 = cheaper than a typical month, above 1 = pricier.
  2. Every home in the shortlist sold in some particular season, so its recorded price already carries that season's effect. Divide the price by the index of the season it actually sold in — that removes the effect and leaves a season-neutral price for that specific home.
  3. Multiply the season-neutral price by whichever season's index you want, and you get what that same home would have cost in that season. Winter vs. spring works out to a 7.1% gap (0.959 vs. 1.033) on every home, which is why the savings column scales with the price of the home.
- We compare against spring deliberately: it's the most expensive season per sqft, so this is the *largest* saving timing can buy her. Against summer it would be 4.4%, against the rest of the year pooled 4.9% — spring is the honest ceiling, not a flattering pick.
- Why $/sqft rather than raw price: if bigger houses happen to sell in summer, a raw-price index would attribute their size to the season. Dividing by square footage first removes that.
- Worth saying plainly if asked: this is a simple index adjustment, not a regression — one factor (season), applied uniformly, with no controls for anything else.
- Every one of the 11 homes comes out cheapest in winter — the discount isn't cherry-picked, it holds across the whole shortlist. Savings vs. spring range roughly $12K–$27K depending on the home.
- Bottom line to leave Nicole with: shop the Seattle shortlist, favor the higher-graded homes at the top of the list, and time the purchase for winter to capture the seasonal discount on top of everything else.

## Closing (say out loud, not a slide)

- This is sold-home data from May 2014–May 2015, not live listings. Use the zip codes and price bands identified here as a filter on today's market — the underlying prices will have moved on.
- We're assuming the seasonal pattern documented here (winter discount, spring peak) still holds today; it's based on one observed year, not a confirmed recurring pattern.
- `sqft_lot15` is a density proxy, not a direct measure of "lively" — things like walkability and nightlife aren't in this dataset at all.
