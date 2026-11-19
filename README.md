# Stock Market Big Data Analytics

**Exploring stock price movements, trading activity, market patterns, and unusual observations using Python and big-data tools.**

## Project overview

This project analyzes a large collection of historical stock-market records with **Polars**, **DuckDB**, **scikit-learn**, **PySpark**, and **Plotly**. It combines data cleaning and feature engineering with SQL-based exploration, unsupervised clustering, and visual analytics.

The notebook includes an interactive exploratory analysis, a K-Means++ clustering workflow, a Spark ML clustering pipeline, and a separate PySpark linear-regression experiment. **Power BI dashboards were part of the overall project, but the `.pbix` file/screenshots were not included in the materials available for this repository package.**

## Tools and technologies

| Tool | How it is used |
| --- | --- |
| Python, Pandas, NumPy | Data handling and feature calculations |
| Polars (including lazy scans) | Reading and transforming large Parquet data |
| DuckDB + SQL | Aggregation, filtering, and querying yearly Parquet partitions |
| scikit-learn | Standardization, K-Means++, silhouette and Davies–Bouldin scores |
| PySpark / Spark ML | Distributed-style feature pipelines, clustering, and regression experiment |
| Plotly | Correlation heatmap, price/volume comparisons, clustering and outlier plots |
| Power BI | Dashboard component (not included in this upload) |

## What the notebook does

1. **Load and explore:** Download a Kaggle dataset; inspect its shape, types, missing values, and descriptive statistics.
2. **Engineer features:** Derive same-day price change, high–low price range, open-to-close return, relative price range, and log trading volume.
3. **Analyze with SQL:** Partition data by year and use DuckDB to summarize prices, volume, and selected trading years.
4. **Visualize:** Explore stock-level averages, correlations, and distributions using Plotly.
5. **Cluster with K-Means:** Scale features and evaluate candidate cluster counts using silhouette, Davies–Bouldin, inertia, and elbow analysis.
6. **Flag unusual records:** Use distance to an assigned cluster centroid to label observations above the 95th-percentile distance threshold as *outliers*.
7. **Experiment with Spark:** Demonstrate a separate Spark ML clustering pipeline and a same-day closing-price regression exercise.

> **Interpretation note:** `Volatility` in the notebook is the **daily high minus daily low**, a range-based proxy rather than annualized statistical volatility. Cluster-distance outliers are exploratory flags, **not confirmed fraudulent events or trading signals**. The regression exercise uses same-day price information and is **not a next-day forecasting model**.

## Data source

**Kaggle:** [9000+ Tickers of Stock Market Data (Full History) — jakewright](https://www.kaggle.com/datasets/jakewright/9000-tickers-of-stock-market-data-full-history)

The dataset includes a `Date` and `Ticker` plus OHLC prices and `Volume`; the available file also includes `Dividends` and `Stock Splits`. A saved output in the original analysis reported **34,646,258 raw rows** and a K-Means cleaning stage with **33,454,003 remaining rows**. These are previously recorded counts, not independently rerun benchmarks.

The dataset and its generated Parquet/CSV intermediates are **excluded from Git** because they are large. Download the dataset yourself and follow its Kaggle usage terms.

## Repository structure

```text
stock-market-big-data-analytics/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
└── notebooks/
    └── Stock_Market_Big_Data_Analytics.ipynb
```

## Getting started

### Option A: Google Colab (recommended for this notebook)

1. Open `notebooks/Stock_Market_Big_Data_Analytics.ipynb` in Google Colab.
2. Run the dependency installation cell.
3. Configure Kaggle API authentication securely; **do not insert a token into the notebook**.
4. Run the Kaggle download/unzip cells. They should create `all_stock_data.parquet` in the current working directory.
5. Run the analysis sections in order, ensuring enough memory and processing time for large data and Spark experiments.

### Option B: Local Jupyter

A Java installation is required for the PySpark portions. From the project root:

```bash
python -m venv .venv
# On Windows: .venv\Scripts\activate
# On macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

Configure your own [Kaggle CLI authentication](https://github.com/Kaggle/kaggle-cli/blob/main/skills/references/auth.md), then switch to the notebook folder so the dataset is downloaded **beside the notebook**:

```bash
cd notebooks
kaggle datasets download -d jakewright/9000-tickers-of-stock-market-data-full-history --unzip
jupyter lab
```

The Python notebook uses a relative file path (`all_stock_data.parquet`). With the commands above, the file belongs in `notebooks/` for local Jupyter. Colab normally uses `/content` as the working directory. You may need to restart your notebook kernel after installing Spark dependencies.

> **Hardware:** Some sections read the full dataset and may require substantial RAM. The automated Spark pipeline has a configurable `MAX_PIPELINE_ROWS = 100_000` sampling limit, but other exploratory cells still load the full Parquet file. For experiments, consider using a smaller subset.

## Outputs and limitations

- The notebook has its saved cell outputs cleared for a smaller, safer GitHub upload. Run the cells to regenerate the charts.
- Raw data, derived datasets, and the Power BI `.pbix` report are not bundled here. A `dashboard/` folder and screenshots can be added once the report is available.
- Outlier labels depend on the chosen feature scaling, clusters, and a 95th-percentile cutoff; this analysis does not establish causal explanations for market events.
- Saved original notebook results were **not fully reproduced or validated** in this packaging step.
- The notebook contains multiple learning/analysis experiments rather than one fully productionized workflow.

## Portfolio summary

Analyzed historical stock-market data with Polars and DuckDB, engineered stock price and trading-volume features, applied K-Means clustering and centroid-distance outlier analysis, and used Plotly and PySpark for exploratory analytics and machine-learning experiments. Power BI dashboard work can be linked when the report is added.

---
*Educational data analytics project. Not investment advice.*
