# NYC Airbnb Data Science Project

Virtual Data Science with Python Trainee — Internship Tasks

## Overview

This project demonstrates data acquisition, cleaning, preprocessing, exploratory data analysis (EDA), and unsupervised learning on a real-world dataset using Python. The dataset is the **New York City Airbnb Open Data (2019)**, originally sourced from Kaggle, containing 48,895 listings across 16 attributes (price, location, room type, host details, review activity, and availability).

## Files

**Week 1 — Data Acquisition, Cleaning, and Preprocessing**
- `clean_data.py` — full cleaning and EDA script (pandas, numpy, matplotlib, seaborn)
- `AB_NYC_2019.csv` — raw input dataset
- `AB_NYC_2019_cleaned.csv` — cleaned output dataset (48,870 rows)
- `Week1_Data_Cleaning_Report.docx` — full written report with methodology, code, and visualizations

**Week 2 — Exploratory Data Analysis and Visualization**
- `eda_analysis.py` — EDA script producing summary statistics, correlations, and 8 visualizations
- `Week2_EDA_Report.docx` — full written report with visualizations, code, and interpretation

**Week 3 — Unsupervised Learning and Clustering Analysis**
- `clustering_analysis.py` — K-Means clustering script (scikit-learn, pandas, matplotlib, seaborn)
- `AB_NYC_2019_clustered.csv` — output dataset with cluster assignments and PCA coordinates
- `Week3_Clustering_Report.docx` — full written report with methodology, code, visualizations, and cluster interpretation

## What the scripts do

**Week 1 — Cleaning**
1. **Initial exploration** — checks shape, dtypes, missing values, and summary statistics
2. **Missing value handling**
   - `name` / `host_name`: filled with `"Unknown"`
   - `reviews_per_month`: filled with `0` (missing = no reviews, not random)
   - `last_review`: converted to datetime; added a `has_reviews` flag instead of imputing a fake date
3. **Duplicate check** — confirmed no exact duplicate rows
4. **Outlier / erroneous entry handling**
   - Removed 11 listings with `price == 0`
   - Removed 14 listings with `minimum_nights > 365`
   - Capped extreme prices using the IQR method (`price_capped` column) instead of deleting them, to preserve sample size
5. **Feature prep** — converted `neighbourhood_group` and `room_type` to categorical dtype
6. **Visualizations** — missing values chart, price distribution, listings by borough, average price by borough

**Week 2 — EDA**
1. **Summary statistics** — price, minimum nights, review activity, and availability
2. **Room type & borough breakdown** — listing counts by category
3. **Visualizations** — price distribution, room type counts, price by room type, correlation heatmap, geographic price scatter, availability vs. reviews, monthly review trend, top neighbourhoods
4. **Correlation analysis** — relationships between price, reviews, availability, and host listing count

**Week 3 — Clustering**
1. **Feature selection & preprocessing** — selects `price_capped`, `minimum_nights`, `number_of_reviews`, `reviews_per_month`, `availability_365`; standardizes with `StandardScaler`
2. **Choosing k** — elbow method (inertia) and silhouette score computed for k = 2 to 10
3. **K-Means clustering** — final model fit with k = 4
4. **PCA visualization** — reduces features to 2D for cluster plotting
5. **Cluster profiling** — mean feature values per cluster, room type and borough composition, geographic distribution

## How to run

```bash
pip install pandas numpy matplotlib seaborn scikit-learn

# Week 1: cleaning
python clean_data.py

# Week 2: EDA (run after clean_data.py, uses its output)
python eda_analysis.py

# Week 3: clustering (run after clean_data.py, uses its output)
python clustering_analysis.py
```

Week 1 outputs: `AB_NYC_2019_cleaned.csv` plus four PNG chart files.
Week 2 outputs: `eda_summary_stats.csv`, `eda_correlation_matrix.csv`, `eda_notes.txt`, plus eight PNG chart files.
Week 3 outputs: `AB_NYC_2019_clustered.csv`, `cluster_profiles.csv`, `clustering_notes.txt`, plus five PNG chart files.

## Key results

**Week 1 — Cleaning**

| Metric | Value |
|---|---|
| Original rows | 48,895 |
| Rows removed (invalid price / minimum_nights) | 25 |
| Final cleaned rows | 48,870 |
| Price outliers capped (IQR method) | 1,328 |

**Week 2 — EDA**

| Insight | Value |
|---|---|
| Median price — Entire home/apt | $160 |
| Median price — Private room | $70 |
| Median price — Shared room | $45 |
| Correlation: price vs. number of reviews | -0.06 |
| Top neighbourhood by listing count | Williamsburg (3,917 listings) |

**Week 3 — Clustering**

| Cluster | Size | Description |
|---|---|---|
| 0 — High-Availability Premium | 25.7% (12,569) | High price ($179), high availability (289 days), low review activity — likely professionally managed |
| 1 — Standard Low-Availability | 59.0% (28,828) | Moderate price ($127), low availability (25 days) — typical occasional host |
| 2 — High-Demand Frequently-Reviewed | 15.0% (7,350) | Highest review count (98) and velocity (3.9/month), low minimum stay — top-performing listings |
| 3 — Long-Minimum-Stay | 0.3% (123) | Extreme minimum stay (245 nights), high price — resembles extended-stay/corporate rentals |

Final clustering model: k = 4, silhouette score = 0.378 (selected via elbow method + silhouette analysis across k = 2–10).

See `Week1_Data_Cleaning_Report.docx`, `Week2_EDA_Report.docx`, and `Week3_Clustering_Report.docx` for full write-ups, including rationale for each decision and its impact on downstream analysis.
