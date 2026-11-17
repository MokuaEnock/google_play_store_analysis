# 📱 Google Play Store App Analysis

### What makes an app successful? Eight research questions on installs, reviews, ratings, price, size, and updates, answered with PySpark, Pandas, SciPy, and Tableau

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-Data%20Processing-E25A1C?logo=apachespark&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-150458?logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-Statistics-8CAAE6?logo=scipy&logoColor=white)
![Tableau](https://img.shields.io/badge/Tableau-Public%20Dashboard-E97627?logo=tableau&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

![Dashboard preview](images/dashboard_preview.png)
<!-- Replace with a screenshot or GIF of your Tableau dashboard -->

**🔗 [View the interactive dashboard on Tableau Public](YOUR_TABLEAU_PUBLIC_LINK)** &nbsp;|&nbsp; **📓 [Read the full analysis notebook](play_store_analysis.ipynb)**

---

## Table of Contents

- [TL;DR](#tldr)
- [Business Problem](#business-problem)
- [Research Questions](#research-questions)
- [Key Findings](#key-findings)
- [Analysis Charts](#analysis-charts)
- [Skills Demonstrated](#skills-demonstrated)
- [Tech Stack](#tech-stack)
- [Dataset](#dataset)
- [Approach](#approach)
- [Data Cleaning Highlights](#data-cleaning-highlights)
- [Statistical Methods](#statistical-methods)
- [Dashboard Walkthrough](#dashboard-walkthrough)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Troubleshooting](#troubleshooting)
- [Limitations](#limitations)
- [Future Work](#future-work)
- [A Note on AI-Assisted Development](#a-note-on-ai-assisted-development)
- [Author](#author)

---

## TL;DR

I cleaned 10,841 raw Google Play Store rows down to **9,659 unique apps** with **PySpark and Pandas**, answered **eight research questions** with rank-based tests, bootstrap confidence intervals, and effect sizes, and built a **multi-page interactive Tableau dashboard**.

**Headline result:** installs and reviews have a very strong positive relationship (Spearman rho = 0.968, 95% CI 0.966 to 0.969). Each 10x increase in installs goes with about 8.2x more reviews, and the pattern holds in all 33 categories with at least 30 apps. Free apps reach far more users than paid apps (median 100,000 vs. 1,000 installs), while category and price type have only small effects on ratings.

> The numbers in this README come from one executed run of the notebook. The notebook's "Research scorecard" and generated "Answer to RQ" cells are the source of truth and regenerate them if the data or cleaning changes.

---

## Business Problem

The Google Play Store hosts millions of apps competing for user attention. Teams deciding what to build, how to price it, and where to invest need evidence about which factors go with strong ratings and high install counts, and they need to know **how strong and how reliable** each relationship is. Public signals such as review counts are easy to see, so this project tests how far they can be trusted.

**Who this is for**

| Audience | What they get from this analysis |
|---|---|
| App developers | Benchmarks for review rate by install tier, which categories rate best, which genres are crowded |
| Product managers | Whether price type, app size, and update recency relate to reach and ratings |
| Investors and publishers | Whether review counts are a reliable traction screen, and where the market is concentrated |
| Marketing teams | Which categories turn installs into reviews and high ratings |

---

## Research Questions

The project began with one question and was extended into eight. Each has a stated hypothesis, a justified method, an effect size with uncertainty, a saved chart, and an **answer generated from the data by the notebook**.

| ID | Question | Method | Effect size reported |
|---|---|---|---|
| **RQ1** | Do apps with more installs get more reviews, and is it a category artifact? | Spearman, Kendall, log-log regression, bootstrap CI, within-category checks, residual outliers | rho, slope, R² |
| **RQ2** | Which categories have the best ratings? | Kruskal-Wallis, bootstrap CI of medians, selection check on unrated apps | epsilon² |
| **RQ3** | Do paid apps earn higher ratings than free apps? | Mann-Whitney U, bootstrap CI, category-adjusted comparison | Cliff's delta |
| **RQ4** | How does app size relate to installs and ratings? | Spearman, size bands, category-adjusted correlation | rho |
| **RQ5** | How does price relate to installs? | Mann-Whitney, Spearman among paid apps, price bands | Cliff's delta, rho |
| **RQ6** | How do installs and ratings vary with update recency? | Spearman, recency bands, share of fresh apps by install tier | rho |
| **RQ7** | Which content-rating groups and genres dominate or are crowded? | Shares, Herfindahl-Hirschman index, saturation index | HHI |
| **RQ8** | Which relationships survive when all factors are considered together? | OLS with category effects, standardized coefficients | R², delta R² |

---

## Key Findings

| RQ | Finding | Effect size |
|---|---|---|
| RQ1 | Very strong positive relationship: Spearman rho = 0.968 (95% CI 0.966 to 0.969). Each 10x more installs goes with about 8.2x more reviews (log-log slope 0.916, R² = 0.919). Positive in all 33 categories with at least 30 apps (lowest rho 0.871), and rho is 0.965 without zero-review apps. Raw-scale Pearson is only 0.625 because a few blockbuster apps dominate. | very strong |
| RQ2 | Highest median ratings: Events, Books & Reference, Health & Fitness (4.5). Lowest: Tools, Maps & Navigation (4.2) and Dating (4.1). Category explains 2.5% of rating rank variance (Kruskal-Wallis p = 1.7e-33). Unrated apps (15.1% of listings) are far less installed (Cliff's delta = -0.88). | small |
| RQ3 | Paid apps average 4.26 vs. 4.17 for free apps (Cliff's delta 0.16, small). After removing category means the delta is 0.14. | negligible after category adjustment |
| RQ4 | Larger apps tend to have more installs (rho = 0.31, weak), the opposite of the hypothesis. The 1M+ install share rises from 13.5% (<5 MB) to 58.0% (60+ MB) and is 73.9% for "Varies with device" apps. Size is unrelated to rating (rho = 0.05). | weak |
| RQ5 | Free apps have a median of 100,000 installs vs. 1,000 for paid apps (Cliff's delta 0.50). Among paid apps, price level has a negligible relationship with installs (rho = 0.13). | large |
| RQ6 | Fresher apps have more installs (rho = -0.33). 78% of 1M+ apps were updated in the last 180 days vs. 49% of apps under 10K installs. The link with rating is negligible (rho = -0.18). | weak |
| RQ7 | `Everyone` apps are 82% of listings and 70% of installs. The top 5 genres hold 29% of apps (HHI 0.036 vs. 0.021 if spread evenly). Larger genres tend to have lower reach per app (rho = -0.28). | descriptive |
| RQ8 | Adding installs lifts R² for log-reviews from 0.12 to 0.91. Rating, price, size, and recency add only 0.02. For installs, category plus those attributes explain R² = 0.21, with size the largest coefficient. | large |

**Recommendations** (generated by the notebook only when the computed effect supports them)
- **Developers:** benchmark your review rate (median reviews per 1,000 installs: 14.0 below 10K installs, 14.1 for 10K to 1M, 20.7 above 1M). Check crowding before entering a genre; Business, Medical, and Education are large genres with low typical reach per app.
- **Product managers:** charging goes with clearly lower reach, so pair a paid model with a free tier or trial. Fresher apps have more installs, so keep a regular update cadence, though the direction of cause is uncertain. Track reviews per install, not just review counts.
- **Investors:** review counts are a strong first screen for traction, but installs are bucketed and the link is correlational, so confirm with other data. Once installs are known, rating, price, size, and recency add almost nothing to explaining reviews.

These findings describe associations in the data, not causes. See [Limitations](#limitations).

---

## Analysis Charts

Each chart below is produced by the notebook and saved to `images/`. They share one seaborn theme (no frame, light gridlines, left-aligned titles, readable 1K / 1M axis labels), and every grouping is shown in its natural order. Together they cover RQ1 to RQ8.

### RQ1. Do apps with more installs get more reviews?

![Distributions of installs and reviews, raw and log scale](images/01_distributions.png)
*Both variables are extremely right-skewed on the raw scale (left) and much more symmetric after a log10 transform (right), which is why log and rank methods are used.*

![Installs vs. reviews on log-log axes with fitted line](images/02_installs_vs_reviews_loglog.png)
*The core relationship: reviews rise steadily with installs, with a fitted log-log line. The call-out gives the slope (each 10x more installs goes with about 8.2x more reviews) and R².*

![Reviews and review rate by install bucket](images/03_buckets.png)
*Left: reviews rise with each install bucket, shown in order from 1+ to 1B+. Right: median reviews per 1,000 installs for apps with 1K+ installs.*

![Installs-reviews correlation within each category](images/04_category_correlation.png)
*Spearman rho inside each category with at least 30 apps, highest first, with the median marked. The relationship holds everywhere, so it is not a category-mix artifact.*

![Spearman correlation matrix](images/05_correlation_matrix.png)
*How installs, reviews, rating, price, size, and update recency relate to each other (Spearman, lower triangle only). Installs and reviews stand out; the other traits are weak.*

### RQ2. Which categories have the best ratings?

![Median rating by category with bootstrap confidence intervals](images/06_category_ratings.png)
*Median rating per category (at least 30 rated apps), shown as a dot with its 95% bootstrap interval, against the overall median. Most intervals overlap, which is why category is a small effect.*

### RQ3. Do paid apps earn higher ratings than free apps?

![Rating distribution for free vs. paid apps](images/07_rating_free_vs_paid.png)
*Rating distributions for free and paid apps, with group sizes and quartiles marked.*

### RQ4. How does app size relate to installs and ratings?

![Share of 1M+ install apps and installs by size band](images/08_size_vs_installs.png)
*Left: the share of apps with 1M+ installs rises with size band. Right: the install distribution per band. Bands run from <5 MB to 60+ MB, with "Varies with device" last as its own band.*

### RQ5. How does price relate to installs?

![Median installs and 100K+ share by price band](images/09_price_vs_installs.png)
*Median installs (log scale) and the share of apps with 100K+ installs, by price band from Free to $10+.*

### RQ6. How do installs and ratings vary with update recency?

![Median installs and mean rating by time since last update](images/10_recency.png)
*Median installs (log scale) and mean rating across recency bands, from updated within 30 days to over two years ago. Ratings are drawn as dots because that axis does not start at zero.*

### RQ7. Which content-rating groups and genres dominate or are crowded?

![Content-rating share and genre size vs. median installs](images/11_audience_and_genres.png)
*Left: share of apps by content rating, from Everyone to Adults only. Right: number of apps per genre against median installs per app, for genres with at least 20 apps; red marks large genres with low reach.*

### RQ8. Which relationships survive in a combined model?

![Standardized coefficients in the combined models](images/12_combined_model.png)
*Standardized coefficients for the reviews model and the installs model (category effects included), so the sizes are comparable. Installs dominate the reviews model.*

---

## Skills Demonstrated

| Skill | How it shows up in this project |
|---|---|
| **Big data processing** | Tuned local SparkSession, explicit schema handling, `cache()` and `persist(MEMORY_AND_DISK)`, `explain()` on the cleaning plan, two window functions (`row_number`, `percent_rank`) plus a `dense_rank` check, `approxQuantile`, `percentile_approx`, session cleanup, and error handling around loading and cleaning |
| **Dependency-safe engineering** | Replaced a Python UDF with native Spark expressions and disabled Arrow, removing the `pyarrow` requirement and fixing a PySpark 4.x failure; an optional UDF cross-check skips itself safely |
| **Data cleaning** | Parsing messy text fields (`1,000+` installs, `$2.99` prices, `19M` sizes), de-duplication, handling malformed rows and nulls, with a row-count audit after each step |
| **PySpark and Pandas together** | PySpark for bulk transformations and profiling, Pandas for dates, features, statistics, and plotting, with a `collect()`-based hand-off |
| **Data quality engineering** | Prioritized, data-driven issue log; type-mismatch counts; helper unit tests; 16 automated validation checks (including ordering checks); CSV export verified by reload |
| **Statistical analysis** | Non-parametric tests, bootstrap confidence intervals, effect sizes (Cliff's delta, epsilon², rho), selection-bias check, category-adjusted comparisons, OLS with NumPy |
| **Data visualization** | Twelve charts built on one custom seaborn theme (shared helpers for titles, log-axis labels and bar labels), followed by a polished Tableau dashboard |
| **Dashboard design** | Multi-page layout, navigation, parameters, filters, and a consistent palette matched to the notebook |
| **Communication** | CRISP-DM structure, hypotheses before results, auto-generated answers, documented assumptions and limitations |

---

## Tech Stack

- **Language:** Python 3.9+ (run tested on Python 3.12)
- **Processing:** PySpark (run tested on 4.3.0), Pandas (2.3.2), NumPy
- **Statistics:** SciPy (Mann-Whitney, Kruskal-Wallis, Spearman, Kendall, regression)
- **Notebook visuals:** matplotlib, seaborn
- **Dashboard:** Tableau Public
- **Environment:** Jupyter Notebook
- **Not required:** `pyarrow` (deliberately avoided; see [Troubleshooting](#troubleshooting))

---

## Dataset

- **Source:** Kaggle, [Google Play Store Apps](https://www.kaggle.com/datasets/yusufdelikkaya/google-play-store-apps-dataset), one row per app listing scraped from the Play Store.
- **Size:** about 4 MB; 10,841 rows × 13 columns (raw), 9,659 rows × 39 columns after cleaning and feature engineering.
- **Snapshot date:** the newest `Last Updated` value in the file is used as "now" when measuring update recency.
- **License / terms:** see the dataset's Kaggle page; check its license before redistributing the raw file.

| Column | Description |
|---|---|
| `App` | Application name |
| `Category` | Store category (34 distinct raw values) |
| `Rating` | Average user rating (1 to 5); missing for 13.6% of raw rows |
| `Reviews` | Number of user reviews |
| `Size` | App size (`19M`, `201k`, or `Varies with device`) |
| `Installs` | Install bucket lower bound (e.g. `1,000+`) |
| `Type` | Free or Paid |
| `Price` | Price in USD |
| `Content Rating` | Target audience group |
| `Genres` | Genre(s), `;`-separated |
| `Last Updated` | Date of the most recent update |
| `Current Ver` / `Android Ver` | App version and minimum Android version (dropped, not analytically useful here) |

**Engineered columns** in the cleaned CSV: `size_mb`, `days_since_update`, `primary_genre`, `is_paid`, `size_missing`, `has_rating`, `log10_installs`, `log10_reviews`, `log10_price1`, `log10_size_mb`, `reviews_per_1k_installs`, `installs_pct_rank_in_category`, `installs_bucket` (with `installs_bucket_rank`), and the ordered groupings `size_band`, `price_band`, `recency_band`, `install_tier`, and `content_rating`. Each ordered grouping also has a numeric `*_sort` column (1 = first in logical order) and, except `content_rating`, a self-sorting `*_ordered` label such as `06 | 1K+`.

---

## Approach

The project follows the **CRISP-DM** framework.

```
Business Understanding → Data Understanding → Data Preparation → Analysis (RQ1 to RQ8) → Dashboard & Reporting
```

1. **Business Understanding:** define the problem, stakeholders, eight research questions, and hypotheses.
2. **Data Understanding:** load with PySpark, profile the data (schema, statistics, nulls, duplicates, type mismatches, distributions, outliers), and log data quality issues in a prioritized table.
3. **Data Preparation:** clean with PySpark, refine with Pandas, validate, and export a clean CSV.
4. **Analysis:** answer RQ1 to RQ8 in the notebook, each with a hypothesis, test, effect size, chart, and generated answer, then collect everything in a research scorecard (`data/research_scorecard.csv`).
5. **Dashboard:** build and publish a multi-page, interactive Tableau Public dashboard, with each view mapped to a research question.

**Why PySpark and Pandas?** PySpark handles loading, profiling, and the bulk transformations (type casts, de-duplication with window functions, size parsing), the work that scales with data size. The cleaned data (9,659 rows) fits comfortably in memory, so Pandas handles date parsing, feature engineering, statistics, and plotting, where its API and the SciPy ecosystem are simpler to use.

**Why no Python UDF?** A Python UDF makes Spark start a Python worker, which on PySpark 4.x imports `pyarrow`. Native Spark expressions (`regexp_extract` plus `when`) run inside the JVM, avoid Python serialization, need no extra package, and are visible to the optimizer. A small UDF is still demonstrated as an optional cross-check against the native expression.

---

## Data Cleaning Highlights

Real-world data is messy. The main issues found and how they were handled:

| Issue | Example | Fix | Rows |
|---|---|---|---|
| Malformed row | `Life Made WI-Fi Touchscreen Photo Frame`: category `1.9`, rating `19`, installs `Free` | Detected (`Installs` not a valid bucket) and removed | 1 |
| Repeated apps | Same app listed multiple times (483 rows are fully identical) | One row per app, keeping the highest `Reviews` then `Installs`, using a `row_number` window | 1,181 |
| Installs stored as text | `"1,000+"` | Stripped symbols, cast to long (treated as a lower bound) | all |
| Reviews stored as text | `"159"` | Cast to long | all |
| Price stored as text | `"$2.99"` | Stripped `$`, cast to double | 800 priced |
| Size in mixed units | `"19M"`, `"201k"`, `"Varies with device"` | Native Spark expression converts to MB; unknown sizes stay null and are flagged with `size_missing` and a `Varies with device` band | 1,695 raw |
| Dates stored as text | `"January 7, 2018"` | Parsed to a date; `days_since_update` measured from the newest date | all |
| Missing ratings | Nulls in `Rating` | **Kept as null, not imputed**; excluded from rating statistics, and RQ2 tests whether unrated apps are less popular | 1,474 raw |
| Missing type | Null in `Type` | Derived from `Price` (0 = Free, >0 = Paid) | 1 |
| Multi-valued genres | `"Action;Adventure"` | Primary (first) genre extracted | 498 |
| Groupings sort alphabetically once exported | `"1B+"` lands before `"1K+"` in a CSV or in Tableau | Ordered categoricals in Pandas, plus `*_sort` columns and `*_ordered` labels in the export; `Varies with device` is always last | all |

The cleaning audit in the notebook shows 10,841 rows in, 10,840 after malformed-row removal, and 9,659 after de-duplication (89.1% kept). Every step and its rationale is documented in the notebook.

### Validation

The cleaned data passes 16 automated checks before export, for example:

```python
assert df["rating"].dropna().between(1, 5).all()
assert (df[["installs", "reviews", "price"]] >= 0).all().all()
assert not df["app"].duplicated().any()
assert df["type"].isin(["Free", "Paid"]).all()
assert (df.loc[df["is_paid"], "price"] > 0).all()
assert df["installs_bucket"].cat.ordered                      # 1K+ sorts before 1M+ before 1B+
assert df["size_band"].cat.categories[-1] == "Varies with device"
```

The exported CSV is reloaded and compared with the in-memory DataFrame (shape, columns, and totals of installs and reviews) to confirm nothing was lost. Helper functions (size parser, effect-size functions, HHI, OLS) have unit-test assertions, and the native Spark size expression is checked against the Python reference parser.

---

## Statistical Methods

Installs, reviews, and prices are extremely right-skewed (skewness of 15.0 for installs and 26.6 for reviews), so the analysis avoids relying on raw means.

- **Rank-based tests** (Spearman, Kendall, Mann-Whitney, Kruskal-Wallis) and **log10(x + 1)** transforms handle the skew and zeros.
- **Effect sizes come first.** With nearly 10,000 apps almost any difference is "significant", so conclusions use Cliff's delta, epsilon², and correlation strength, not p-values.
- **Uncertainty** is shown with bootstrap 95% confidence intervals (Spearman's rho, category medians, and the free-vs-paid mean rating difference).
- **Confounding checks:** within-category and category-demeaned comparisons rule out a category-mix explanation (RQ1, RQ3, RQ4).
- **Selection check:** RQ2 first tests whether unrated apps differ in popularity, because rating results only describe apps that attracted enough reviews.
- **Robustness:** zero-review apps are excluded in one check; apps under 1,000 installs are excluded from review-rate comparisons because their rates are noisy.
- **Combined model (RQ8):** OLS with category fixed effects and standardized predictors, written with NumPy, reports how much R² each block adds.

---

## Dashboard Walkthrough

🔗 **[Open the live dashboard](https://public.tableau.com:9443/views/GooglePlaystoreAppAnalysis_17946942766570/GoogleAppAnalysis?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)**

| Page | What it shows | Research question |
|---|---|---|
| **1. Overview** | KPI tiles (apps, total installs, median rating, median review rate) and the installs vs. reviews scatter on log axes with a trend line | RQ1 |
| **2. Reviews and Installs** | Median reviews and review rate by install bucket, outlier table of apps far above or below the trend | RQ1 |
| **3. Ratings** | Median rating by category, free vs. paid rating distribution | RQ2, RQ3 |

**Interactivity:** filters for `category`, `type`, `content_rating`, and `install_tier`; a parameter that switches the metric (reviews, rating, review rate) and sets a minimum-installs threshold; navigation buttons between pages.


**Design decisions:** consistent palette across every page (green for installs, blue for reviews, red for highlights), matching the notebook charts. A CSV loses category order, so install buckets and every band field are sorted by their matching `*_sort` column (or use the `*_ordered` labels), which keeps 1K+ before 1M+ before 1B+ and puts `Varies with device` last. Log axes are used on the scatter because both variables are heavily skewed, and medians replace averages for the same reason.

---

## Repository Structure

```
play-store-analysis/
├── README.md
├── play_store_analysis.ipynb        # Full CRISP-DM analysis (RQ1 to RQ8)
├── data/
│   ├── google_play_store_dataset.csv        # Raw dataset (see Dataset section)
│   ├── google_play_store_cleaned.csv        # Cleaned export, the Tableau data source
│   └── research_scorecard.csv               # Generated answers to RQ1 to RQ8
├── images/                          # Notebook charts (01 to 12), dashboard screenshots, banner
├── docs/
│   └── ai_usage_log.md              # Prompts, responses, and my modifications
├── requirements.txt
└── .gitignore
```

If you keep raw and processed data in separate folders, change `raw_path` and `clean_path` in the notebook's configuration cell.

---
<!--
## Getting Started

### Prerequisites
- Python 3.9+
- Java 17 or 21 for PySpark 4.x (Java 8 or 11 for PySpark 3.x); check with `java -version`

### Run the analysis

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/play-store-analysis.git
cd play-store-analysis

# 2. (Recommended) create a virtual environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 3. Install dependencies (pyarrow is NOT needed)
pip install -r requirements.txt

# 4. Add the raw dataset
#    Place the CSV at data/google_play_store_dataset.csv

# 5. Launch Jupyter and run all cells
jupyter notebook play_store_analysis.ipynb
```

Suggested `requirements.txt`:

```
pyspark
pandas
numpy
scipy
matplotlib
seaborn
jupyter
```

Running the notebook regenerates `data/google_play_store_cleaned.csv` (the data source for the Tableau dashboard), `data/research_scorecard.csv`, and the charts in `images/`.

--- -->

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| `ModuleNotFoundError: No module named 'pyarrow'` during cleaning | A Python UDF started a Python worker that imports `pyarrow` on PySpark 4.x | Already handled: the notebook uses native Spark expressions and has Arrow disabled. If you add your own UDFs, either `pip install pyarrow` or keep logic in native expressions |
| Spark workers use a different Python than the notebook | Virtual environments or pipx installs | The notebook sets `PYSPARK_PYTHON` to the current interpreter before starting Spark |
| `Dataset not found` | Wrong path | Put the CSV at `./data/google_play_store_dataset.csv` or edit `raw_path` |
| Java errors on startup | Unsupported Java version | Install Java 17 or 21 for PySpark 4.x |
| Spark errors after running the last cell | The final cell calls `spark.stop()` | Re-run the notebook from the top if you need Spark again |
