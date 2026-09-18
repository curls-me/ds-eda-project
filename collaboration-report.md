# ds-eda-project — collaboration retrospective

A look back at three days of work on the King County house-sales analysis, based on the decision log for 2026-09-16 through 2026-09-18.

---

## 1. How the problem was framed

**The assignment.** A bootcamp exploratory data analysis (EDA) project using King County (Seattle area) house-sales records from May 2014 to May 2015. The deliverable was not just an analysis — it was a recommendation to a hypothetical client, backed by evidence and a short presentation.

**The client.** "Nicole Johnson," a persona with concrete constraints: a 1–2 bedroom home of roughly 45–60 square metres (converted early on to 485–645 sqft, since the dataset is in imperial units), in a central and lively neighbourhood, at a mid-range price. Every hypothesis test in the project was eventually narrowed to *her* slice of the market rather than the market as a whole — which turned out to matter a great deal, because patterns that hold for large family homes do not necessarily hold for small units.

**The three hypotheses.**

1. **Downtown premium** — houses within 5 miles of a major downtown have a median price at least 50% higher than houses further out.
2. **Density** — neighbourhoods where nearby homes sit on smaller lots are denser and more "urban-lively," which is a proxy for what Nicole says she wants.
3. **Seasonality** — sales volume peaks in summer, and homes bought in winter cost around 5% less.

**How the collaboration was set up.** You ran this as a review-gated workflow rather than letting Claude edit your working copy directly. Each piece of work happened in an isolated copy of the repository, was pushed as a pull request, and you reviewed and merged it yourself on GitHub — twelve PRs in total. Only twice did that pattern bend: once when you explicitly pre-authorised Claude to merge because you were away, and once on the final day when you authorised small copy edits to go straight to the main branch. The effect was that no analytical conclusion entered the project without you seeing it first, which is the main reason the several reversals described below were caught rather than shipped.

---

## 2. How the work unfolded

### Day 1 (2026-09-16) — Build the notebook, test all three hypotheses

Seven sessions, and by the end of the day all three hypotheses had been tested and every one of them had come back more complicated than stated.

**Getting the data in shape.** The first session filled in the course's existing notebook template rather than starting fresh, then worked through a standard cleaning checklist: four columns with missing values were filled in (one of them derived exactly from an arithmetic identity in the data rather than guessed), one clearly-broken record was dropped — a house listed with 33 bedrooms in 1,620 square feet — and 177 records that shared an ID were deliberately *kept*, because they are legitimate re-sales of the same house rather than duplicates. Net result: 1 row lost out of 21,597, about 0.005%. Claude also diagnosed a "notebook hangs forever" problem as two orphaned background processes rather than a code bug, which saved chasing the wrong thing.

*What Claude proposed on its own:* keeping every row via transformation rather than deleting outliers; adding a flag to preserve the "was renovated" signal before zeroing out a date column. You directed the cleaning checklist itself and asked that everything be added as runnable cells you could execute yourself, not pasted-in results.

**Hypothesis 1 failed on first contact.** Tested across Nicole's 1–2 bedroom slice, pooling all five downtowns together, the downtown premium was **+2.9% and statistically insignificant**. The hypothesis, as written, simply did not hold. Rather than stopping there, Claude proposed escalating: Seattle alone (+23.9%, real but well under the 50% bar), then a full city-by-city breakdown. That revealed the pooled number was hiding two opposite effects — Bellevue and Seattle carried a premium, while **Renton, Kent and Federal Way were actually cheaper than the outskirts baseline**, the reverse of the hypothesis. The headline shifted from "downtown is expensive in King County" to "a downtown premium exists in two specific cities and nowhere else."

**Hypothesis 3 reversed itself.** Sales volume does vary by season — but the peak is **Spring at 30.2%**, not Summer at 29.3%, so the claim as stated was wrong by a hair but wrong nonetheless. On price, winter homes were **5.5% cheaper**, matching the claimed magnitude. Claude then proposed a follow-up check controlling for house size, quality and location, and that check dissolved the finding: the winter effect shrank to **under 1% and lost significance**. The plain-English reading was that winter doesn't discount houses; winter just happens to be when smaller homes in cheaper zip codes change hands.

**Hypothesis 2 and the first combined recommendation.** You asked for a "mid-price band" definition per downtown and picked the interquartile range (the middle 50% of prices) from a set of options. Density was then tested with deliberately the same statistical method as Hypothesis 1, so the two results could be compared fairly, and the two were joined into a single recommendation table flagged "recommended for Nicole" — passing only if a city cleared *both* the price-premium bar and the density bar. **Only Seattle cleared both.** Bellevue was significantly pricier but not significantly denser: central, but not lively by this measure.

**Cleaning up the analysis itself.** In the sixth session you asked whether a mathematical transformation applied on day one was even needed. Instead of answering in the abstract, Claude checked where it was actually used — it fed exactly one calculation, and three of its four outputs were never referenced anywhere. It was removed, which simplified the notebook and let one result be reported in dollars instead of approximate percentages. Claude caught a bug this introduced (a library import that lived only inside the deleted section) before it was committed.

**The pivot away from "must be expensive."** You raised the methodological objection yourself, and it was the sharpest call of the project: requiring a city to be *both* significantly pricier *and* significantly denser was punishing exactly the outcome Nicole would want. A dense neighbourhood that happens to be cheap is a **better** match, not a weaker one. The shortlist was rebuilt on density alone, with a single price ceiling pooled across qualifying towns rather than one band per town — because "cheap for Seattle" is ordinary money in Kent, and a per-town band would have wrongly excluded genuinely affordable Seattle homes. The shortlist grew from **9 homes to 11**, with a pooled price ceiling of about $383,944.

### Day 2 (2026-09-17) — Simplify, then build the presentation

**Replacing a technique that was ahead of the course.** You flagged that the statistical control used on Hypothesis 3 hadn't been covered in the bootcamp yet. Claude replaced it with a much simpler like-for-like comparison — price *per square foot* by season, which normalises for size without needing a model. The result: winter is **4.9% cheaper per square foot and still significant**, versus 5.5% on raw price. This *reinstated* the winter discount that day 1's more advanced check had dissolved. It also caught that the notebook's own written takeaway still said "Summer" was the peak season when its own test had already shown Spring — a small but embarrassing inconsistency, fixed.

Four scattered "this is historical data, not live listings" disclaimers were consolidated into one upfront assumption. And a closing recommendation section was added: the 11 shortlisted homes ranked by build quality, each one's price projected across all four seasons by backing out the season it actually sold in and reapplying each target season. **All 11 came out cheapest in winter, saving $7,000–$16,000 versus summer.** That turned a dataset-wide average into a per-home number Nicole could act on.

**Keeping the story in sync.** The presentation outline had been written before several of these findings changed. Claude compared it against the notebook and found three stale passages — including one that described the old "clears both bars" logic and another that said the winter discount had disappeared, which by then was the *opposite* of the current finding. Because these were reversals of conclusions and not just number updates, Claude presented an explicit before/after plan and waited for approval before editing. You approved it as proposed.

**Building the deck.** You named the three charts you wanted; one of them (price per square foot by season) didn't exist yet, so it was added to the notebook in the same visual style as the existing charts, and the whole notebook was re-run end to end to confirm no prior result had shifted. All slide images were pulled from the notebook's own executed output rather than re-plotted separately, so the deck cannot quietly drift from the analysis. Claude also screenshotted the finished deck rather than trusting the code — which caught a chart legend cut off at the edge of a slide that a code review would have missed. You asked for a plain-markdown copy of the slide text alongside the HTML so you could edit wording without touching markup, which proved useful immediately.

**Your edits, folded back in.** Between sessions you edited that markdown directly — merging two slides, cutting one, and specifying additional tables and charts. Those edits were absorbed into a rebuilt deck, the slide numbering was fixed, and the longer methodology talking points were split into a separate speaking-notes file so the slides stayed terse. Claude also noticed the 11-home table was labelled "ranked by build quality" but wasn't actually sorted that way, and fixed the sort to match its own caption.

### Day 3 (2026-09-18) — The correction that mattered most

The day started with a two-minute request: add a downtown-versus-outskirts price column to slide 2. While doing it, Claude flagged — unprompted — that the "outskirts" figure being shown was a **single pooled number of $460,000 repeated identically for all five cities**. That isn't a formatting problem; it means each city was being compared against a county-wide leftover group rather than against its own surroundings. Comparing downtown Kent to a pool dominated by rural and exurban King County is not the comparison a buyer cares about.

You agreed ("good point") and asked for a proper per-city version, added alongside the original test rather than replacing it. Every house was assigned to its nearest downtown, and each city's downtown was compared to **that same city's own local outskirts**.

The qualitative conclusion held — Bellevue and Seattle are still meaningfully pricier than their surroundings; Renton, Kent and Federal Way still are not — but the magnitudes changed substantially:

| | Old (pooled outskirts) | New (local outskirts) |
|---|---|---|
| Bellevue | +82.7% | **+31.9%** |
| Seattle | +23.9% | **+29.1%** |

The consequence for the presentation was direct: **under the fairer comparison, neither city clears the hypothesis's 50% bar**, whereas Bellevue had previously appeared to clear it comfortably. A claim that had been on a slide for two days was no longer true.

The corrected numbers were then propagated through the deck, and a repo-wide search for the old figures found the outline, slide markdown and speaking notes all still carrying the stale numbers and the now-false "Bellevue clears the 50% bar" line. All three were updated, and the day ended with a clean, fully consistent repository.

---

## 3. The throughline: four times a finding got walked back

This project's real story is that **every one of the three hypotheses turned out to be more fragile than it first looked, and each was corrected before it reached the client-facing deliverable.**

1. **Pooled vs. per-city downtown premium (day 1).** The pooled test said +2.9% and insignificant — the hypothesis had failed. Breaking it down per city showed it hadn't failed so much as been wrongly aggregated: two cities had a premium, three were *cheaper* than the baseline. Without that breakdown the honest report would have been "no effect," which would have been misleading in the other direction.

2. **Seasonality, reversed and then re-reversed (days 1 and 2).** The raw 5.5% winter discount survived a simple test, collapsed to under 1% under a statistical control, and then came back at 4.9% under a simpler size-normalised comparison. The middle step is the interesting one: the control showed that a naive "winter is cheaper" claim is partly a composition effect, and the final method kept the conclusion while staying inside what the course had taught. Both of the first two versions had already reached the presentation outline before being corrected.

3. **"Clears both bars" to density-only (day 1, session 7).** The original recommendation logic required a city to be both significantly more expensive and significantly denser. That quietly penalised affordability — the one thing a buyer wants. Dropping the price-premium requirement and switching to one pooled price ceiling grew the shortlist from 9 homes to 11 and, more importantly, made the shortlist mean what it claimed to mean.

4. **The local-outskirts correction (day 3).** The largest single swing in the project — Bellevue's premium fell from +82.7% to +31.9% — and it came from a question nobody had asked, raised while doing unrelated cosmetic work. It changed the final verdict on Hypothesis 1 from "partly supported, Bellevue clears it" to "not supported at the stated 50% threshold anywhere."

The pattern is consistent and worth naming: in every case the first-pass number was directionally plausible and the refined comparison was materially different. The project's value came less from running the tests than from repeatedly asking whether the comparison being made was the right one.

---

## 4. Where it landed

The notebook and deck now say, in plain terms:

- **Hypothesis 1 (downtown premium): not supported as stated.** Across small 1–2 bedroom homes, there is no 50%+ county-wide downtown premium. Compared against their own local surroundings, Bellevue runs about **+31.9%** and Seattle about **+29.1%** — real and statistically meaningful, but short of the 50% bar. Renton, Kent and Federal Way are cheaper downtown than out, the opposite of the claim.
- **Hypothesis 2 (density): supported, and it drives the recommendation.** Using neighbouring lot sizes as a proxy for urban density, **only Seattle qualifies as genuinely dense** among the five downtowns. Bellevue is central and expensive but not dense — so it fits "expensive" without fitting "lively."
- **The recommendation: Seattle, with an 11-home shortlist.** Eleven homes match Nicole's size, budget and density criteria, ranked by build quality, all within a pooled price ceiling of roughly $383,944.
- **Hypothesis 3 (seasonality): half right, and actionable.** Sales peak in **Spring (30.2%)**, not summer. Winter homes are about **4.9% cheaper per square foot**, which holds up once size is accounted for. Projected across the shortlist, buying in winter rather than summer saves **$7,000–$16,000 per home** — the concrete "buy in winter" advice that closes the deck.
- Supporting materials: the notebook as the single source of truth, an HTML deck, an editable markdown copy of the slide text, a presentation outline, and a separate speaking-notes file with the methodology caveats (5-mile radius, straight-line rather than driving distance, lot size as an imperfect density proxy).

---

## 5. What went well

- **Limitations were flagged before they became problems.** The day-3 outskirts issue is the clearest case: a routine formatting request turned into the project's most consequential correction because the comparison itself was questioned rather than just reformatted. Similarly, on day 1 the pooled-test failure prompted a per-city breakdown instead of a shrug.
- **Claims were verified by running things, not by reading them.** Repeatedly — executing the full notebook end to end before committing, extracting the dependency chain and running it against the real dataset to get the actual shortlist numbers before reporting them, re-running everything after adding a chart to confirm no prior result had moved. This is the difference between "the code looks right" and "the number is right."
- **Reversals were surfaced as decisions, not silently patched.** When the outline contradicted the notebook because conclusions had flipped, you got an explicit before/after plan and approved it before anything was edited. That is exactly the right handling — a number moving is a sync job, a conclusion flipping is a decision.
- **One source of truth.** Charts and tables for the deck were pulled from the notebook's own executed output rather than regenerated, so the slides physically cannot disagree with the analysis. The day-3 audit — searching the whole repository for the old figures — is the same instinct applied after the fact, and it caught three files still carrying dead numbers.
- **Visual checking of the deck.** Screenshotting the rendered slides caught a clipped chart legend that no amount of code review would have found.
- **Self-consistency checks.** Noticing that a table captioned "ranked by build quality" wasn't sorted that way, and that a takeaway said "Summer" when its own test said Spring — small catches, but exactly the kind of thing that erodes credibility in a presentation.
- **Safety boundaries held.** On several occasions an action was declined or blocked (merging without review, deleting a file, dropping a saved copy of your edits) and handed back to you rather than worked around.

## 6. What could be improved

- **The unprompted Scibloom tie-in.** While explaining a statistical technique, Claude volunteered a "how this connects to Scibloom" section that you hadn't asked for and didn't want — this was bootcamp work. You corrected it and it was saved as a standing rule for this project. Worth noting as the one clear case of scope creep: extra material added on assumption rather than request.
- **The same data-file problem, four times.** The raw CSV is deliberately excluded from version control (per the assignment), which means every fresh isolated working copy starts with no data and nothing can be executed. This bit on day 1 sessions 5, 6 and 7 and again on days 2 and 3 — twice surfacing as *you* hitting the error ("I cannot load the data on this branch") rather than it being anticipated. It was solved the same way every time by copying the file in. After the second occurrence it should have become a standing first step, not a recurring surprise.
- **Your local copy repeatedly fell behind the shared one.** This caused real confusion at least three times: a phantom "uncommitted change" that turned out to be a branch pointer stuck 7 commits back; a work session unknowingly building on a base 8 commits ahead of yours; and on day 3, you opening a file and seeing the *old* table because the update had been pushed to the shared copy without refreshing yours. Each was diagnosed correctly and fixed non-destructively, but the same failure recurring across three days points at a missing habit — sync your local copy at the end of every session — rather than three unrelated incidents.
- **Inconsistent cleanup of finished work.** Merged branches were sometimes deleted immediately, sometimes left "for the user to decide," with the convention changing mid-project. The result was that day 3's final audit turned up two abandoned working copies and a stale branch left over from *earlier days* — harmless, but a housekeeping debt that accumulated because the rule kept changing. One consistent rule from the start would have avoided it.
- **Findings made it into the deck before they were stable.** Twice — the seasonality conclusion and the Bellevue 50% claim — a finding was written into the outline or slides and then had to be walked back and re-propagated across three or four files. This wasn't wrong exactly (the analysis genuinely improved), but it does suggest the presentation was started slightly early relative to how settled the analysis was. A short "which conclusions are still soft?" check before writing slides would have reduced the rework.
- **A leftover safety artefact.** On day 2, a copy of your superseded local edits was saved as a precaution and then couldn't be cleaned up because the removal was flagged as irreversible. It's still sitting there. Minor, but it's the kind of thing that's easy to forget about and confusing to rediscover months later.
