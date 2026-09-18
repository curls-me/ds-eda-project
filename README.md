# King County Housing EDA — Finding Nicole's Home

Exploratory data analysis of **21,597 home sales in King County (Seattle area), May 2014 – May 2015**, turned into a concrete, evidence-backed shortlist for one client.

Bootcamp project. The dataset lives in the `eda` schema of a PostgreSQL database as two tables; the analysis joins them, cleans them, tests three hypotheses, and ends with a ranked list of homes the client should actually go and look at.

---

## The client

**Nicole Johnson** (buyer, from the brief's client list): wants a **lively, central** neighbourhood, **middle price range**, buying **within a year**. Narrowed to a concrete search profile: a single first-time buyer, no kids, looking for a **1–2 bedroom home of 485–645 sqft** (45–60 m²).

Every hypothesis was tested on *her* slice of the market — small units — not on the market as a whole. That mattered: patterns that hold for large family homes do not hold for small ones.

## What I did

**1. Turned vague client language into testable definitions.** "Lively and central" isn't a column in the data, so it had to be defined explicitly and stated up front:

- **Central** = within 5 miles (straight-line/haversine) of one of the five biggest cities — Seattle, Bellevue, Kent, Renton, Federal Way. Beyond 5 miles from all five = outskirts. The radius is my choice, not the data's.
- **Lively** = dense, proxied by `sqft_lot15` (average lot size of a home's 15 nearest neighbours). Nothing in the data measures walkability, nightlife or commute time, so this is a proxy, not a measurement.
- **Mid-price range** = the interquartile range (middle 50%) of prices for her size band in each downtown.

**2. Cleaned the data without throwing it away.** Four columns with missing values were filled rather than dropped (`yr_renovated` 17.8%, `waterfront` 11.1%, `sqft_basement` 2.1% — derived exactly from `sqft_living − sqft_above` — and `view` 0.3%). A "was renovated" flag was added before zeroing the date column, so the signal survived. 177 records sharing an ID were **kept** — they are genuine re-sales of the same house, not duplicates. One clearly broken record was dropped (33 bedrooms in 1,620 sqft). Net loss: **1 row out of 21,597 (0.005%)**.

**3. Tested three hypotheses, and let the data overturn two of them.**

| # | Hypothesis | Result |
|---|---|---|
| 1 | Downtown homes are 50%+ pricier than the outskirts | **Rejected as stated.** Pooling all five downtowns hid the effect entirely (p = 0.46). Tested city-by-city against each city's *own* local outskirts: Bellevue +31.9% and Seattle +29.1% (both significant, both under the 50% bar); Renton −15.4%, Kent −9.0%, Federal Way −12.7% — *cheaper* than their outskirts, and not significant. |
| 2 | Downtowns are denser than the outskirts | **Partly confirmed.** Downtown overall is 7.7% denser (significant), but only one city drives it: Seattle's neighbouring lots are **45.4% smaller**. Kent, Renton and Federal Way show no significant difference; Bellevue's lots are 26% *bigger*. |
| 3 | Summer is the sales peak, winter is ~5% cheaper | **Half wrong, half confirmed.** The peak is **Spring (30.2%)**, not Summer (29.3%) — the claim is wrong by a hair. Winter sales were **5.5% cheaper**, and the discount survives controlling for house size: **4.9% cheaper per sqft**, with winter running **7.1% below** spring. |

**4. Built the shortlist.** Filtering on Nicole's size band inside the one downtown that passed the density test (Seattle), at or below the 75th percentile of that band's prices, gives **11 matching homes**. They're ranked by build quality (`grade`, then `condition` as a tiebreaker) and re-priced for each of the four seasons, so the same home can be compared winter vs. spring.

## Insights

1. **The "downtown premium" is two cities, not a region.** Only Bellevue and Seattle carry a real premium over their own outskirts, and neither reaches the 50% assumed. Three of the five downtowns are *cheaper* than their surroundings — pooling them together cancels the effect out and hides both stories.
2. **Geographical: Seattle is the only genuinely dense downtown.** Its neighbouring lots are 45.4% smaller than the outskirts. Bellevue is pricier but not denser — central, but not "lively" by this measure. Since a lively-but-cheaper downtown is a *good* outcome for a buyer, density, not price, is the criterion that decides where she should look.
3. **The winter discount is real, not a composition effect.** Winter homes sold 5.5% below the rest of the year, and the gap holds at 4.9% once measured per square foot — so it isn't simply that smaller houses happen to sell in winter.

## Recommendations for Nicole

1. **Search in Seattle.** It's the only downtown that is genuinely denser (and so "lively") than its outskirts. Realistic budget for her size range: **$280K–$384K**.
2. **Start with the 11 matching homes**, highest grade and condition first — every one already meets her size, location, density and budget criteria.
3. **Buy in winter.** The discount holds after controlling for house size and is worth roughly **$12K–$27K** per home against buying at the spring peak.

**Caveats:** this is sold-home data from May 2014 – May 2015, not live listings — use the zip codes and price bands as a filter on today's market. `sqft_lot15` is a density proxy, not a measure of liveliness. The seasonality is one observed year, not a confirmed recurring pattern.

---

## Repository contents

### My work

| File | Description |
| ---- | ----------- |
| [**04 - EDA notebook**](04_eda.ipynb) | **The main deliverable.** Full analysis: loading, descriptive statistics, distributions, assumptions, hypotheses, cleaning, the three hypothesis tests, and the final shortlist — with commentary and takeaways throughout. |
| [**03 - Fetching the data**](03_fetching_the_data_eda.ipynb) | Connects to the PostgreSQL database with psycopg2/SQLAlchemy, joins the two `eda` tables and writes the combined dataset to `data/`. |
| [**Presentation slides (source text)**](presentation_slides.md) | The slide copy: cover plus 7 content slides, written for a non-technical audience. Editing this file is how the presentation changes. |
| [**Presentation (HTML)**](presentation.html) | The rendered, full-screen slide deck built from the slide copy above. Open it in a browser. |
| [**Presentation outline**](presentation_outline.md) | The planning document behind the deck — SCR structure (Situation / Complication / Resolution), slide budget, and a checklist cross-check. |
| [**Slide assets**](slide_assets/) | Charts exported from the notebook for the deck: [downtown map](slide_assets/map_downtowns.png), [density boxplot](slide_assets/density_boxplot.png), [sales by season](slide_assets/sales_by_season.png), [price per sqft by season](slide_assets/price_per_sqft_season.png). |
| [**Collaboration retrospective**](collaboration-report.md) | A write-up of how the three days of work actually unfolded, including the hypotheses that reversed and why. |

### Brief and reference material

| File | Description |
| ---- | ----------- |
| [**01 - Assignment**](01_assignment.md) | The project brief: dataset, tasks, deliverables and the client list. |
| [**02 - Workflow**](02_workflow.md) | The recommended EDA workflow this project followed. |
| [**Column names**](column_names.md) | Data dictionary for the King County housing dataset. |
| [**data/**](data/) | Where the dataset CSV is saved. The folder is tracked; the data files are deliberately kept out of git. |
| [**.env.example**](.env.example) | Template for the database credentials. Copy to `.env` and fill in. |
| [**pyproject.toml**](pyproject.toml) / [**uv.lock**](uv.lock) | Dependencies and lock file. |

---

## Running this yourself

### 1. Clone and install

```bash
git clone git@github.com:curls-me/ds-eda-project.git
cd ds-eda-project
uv sync
```

This installs all dependencies into a virtual environment in `.venv/`. To add a library: `uv add <package-name>`, then commit `pyproject.toml` and `uv.lock`.

### 2. Set up database credentials

```bash
cp .env.example .env
```

Fill in the credentials for the King County housing database (the same ones used in DBeaver). These feed [03 - Fetching the data](03_fetching_the_data_eda.ipynb).

> [!CAUTION]
> `.env` holds secrets and must never be committed. It is already in `.gitignore`. Only `.env.example`, with placeholders, belongs in the repository.

### 3. Open the notebooks

Launch VS Code from the project root so it picks up the uv environment:

```bash
code .
```

Then select the `.venv` interpreter as the notebook kernel. Run [03 - Fetching the data](03_fetching_the_data_eda.ipynb) first to produce the CSV in `data/`, then work through [04 - EDA](04_eda.ipynb).

---

## References

- [**House Sales in King County dataset**](https://www.kaggle.com/datasets/harlfoxem/housesalesprediction) — the source dataset and its column descriptions.
- [**Pandas user guide**](https://pandas.pydata.org/docs/user_guide/index.html) — data manipulation.
- [**Seaborn tutorial**](https://seaborn.pydata.org/tutorial.html) — statistical visualisation.
- [**SQLAlchemy documentation**](https://docs.sqlalchemy.org/en/20/) — querying PostgreSQL from Python.
- [**EDA Checklist**](https://github.com/neuefische/datascience-infographics/blob/main/EDA_Checklist.md) — phase-by-phase checklist.
- [**Detailed EDA with Python**](https://www.kaggle.com/code/ekami66/detailed-exploratory-data-analysis-with-python) — a worked example of a thorough EDA notebook.
- [**Tips for data science presentations**](https://www.dataknowsall.com/storytelling.html) — storytelling for a non-technical audience.
