# StoreCast – Resume & Interview Cheat Sheet

GitHub:https://github.com/dakshvanshaj/StoreCast

Docs:https://dakshvanshaj.github.io/StoreCast/

## Resume Bullets (Quantified Impact)

  

- **Engineered a production-grade Medallion Lakehouse** using **PySpark, Polars, and DuckDB**, processing **2.45B in annual revenue data** (476k+ rows) and reducing manual demand planning labor by **320 hours/month**.

- **Reduced Weighted Mean Absolute Percentage Error (WMAPE) from 11.85% to 7.76%** using an **XGBoost** regressor optimized via **Optuna** Bayesian search, representing a **35% relative accuracy improvement** over manual heuristics.

- **Triggered a $20.53M release of trapped working capital** by enabling a **9.5% reduction in safety stock buffer** through high-precision granular forecasting (4,455 individual Store-Department time series).

- **Projected $9.61M in annual bottom-line profit growth** by slashing inventory holding costs by **$4.1M** and preserving **$5.5M** in gross margin through localized, elasticity-driven markdown optimization.

- **Architected a context-aware Anomaly Detection system** using **Isolation Forests** on dimensionless ratios (YoY growth, trend deviation), eliminating volume-bias "alert fatigue" and precisely flagging operational failures.

- **Designed a de-seasonalized Market Basket Analysis pipeline** using **Pearson Correlation on residuals**, successfully isolating true causal cross-selling affinities by stripping out confounding Black Friday/Holiday spikes.

- **Implemented a zero-cloud-lock-in MLOps framework** using **DVC** for data versioning, **DagsHub/MLflow** for experiment tracking, and **uv** for deterministic dependency management, ensuring 100% pipeline reproducibility.

- **Automated store segmentation** using **K-Means clustering (K=3)** and the Elbow Method, mapping 45 stores into actionable archetypes (Supercenter, Standard, Express) for targeted marketing ROI.

- **Secured mathematical pipeline integrity** by integrating **Great Expectations** data contracts and a custom **PyTest** suite (80%+ coverage on core logic), preventing data leakage via strict chronological cross-validation.

  

## Skills and Tech Stack

  

### Languages & Core

- **Primary:** Python (3.13), SQL (DuckDB dialect), Rust (via Polars internals).

- **Features:** Vectorized processing, Functional programming (Polars Lazy API), Object-Oriented ML wrappers.

  

### Data & Storage

- **Primary:** Delta Lake (ACID-compliant storage), DuckDB (OLAP engine), Polars (Rust-based transformations).

- **Secondary:** Apache PySpark (Bronze ingestion), Apache Arrow, Parquet (Dictionary encoding & Snappy compression).

  

### ML / Analytics

- **Primary:** XGBoost, Scikit-learn (TransformedTargetRegressor), Optuna (Bayesian Optimization).

- **Analysis:** SHAP (Explainability), Isolation Forest (Anomaly), K-Means (Segmentation).

- **Metrics:** WMAPE (Weighted Mean Absolute Percentage Error), Silhouette Score, Pearson Correlation Residuals.

  

### Orchestration & Infrastructure

- **Primary:** DVC (Data Version Control), DagsHub (Remote Storage & MLflow Tracking), `uv` (Fastest Python Resolver).

- **Secondary:** Kubernetes (Serving), MkDocs (Documentation), GitHub Actions (CI/CD).

  

### Tooling & Practices

- **Primary:** Pytest, Great Expectations (Data Contracts), Structlog (Structured Logging).

- **Practices:** Medallion Architecture (Bronze/Silver/Gold), OBT (One Big Table) Denormalization, Chronological Splitting.

  

## Design Decisions and Trade-offs

  

- **Hybrid Compute (Spark + Polars):** Chose PySpark for **Horizontal Scale** (Bronze ingestion) to handle unbounded raw data, but switched to Polars for **Vertical Speed** (Silver transformation) to eliminate expensive Spark "Network Shuffles" and reduce startup latency.

- **OBT (One Big Table) Gold Layer:** Opted for a completely denormalized Gold layer over a traditional Star Schema. **Trade-off:** Slightly higher storage footprint (negated by Parquet dictionary encoding) vs. **Zero-Join runtime** for ML training and Dashboarding.

- **DVC over dbt:** Used DVC for pipeline orchestration to maintain a **Python-native ML environment** and keep compute local/decoupled, avoiding the need for an expensive cloud-managed Data Warehouse required by dbt.

- **Log Transformation (`log1p`):** Applied log transformation to the target variable (`weekly_sales`) to handle fat-tailed distributions and zero-bounded retail data, using Scikit-Learn wrappers to automate the inverse transform (`expm1`) for business-ready dollar predictions.

- **Pearson Correlation on Residuals:** Rejected raw correlations for Market Basket analysis to avoid **Seasonality Bias** (e.g., Turkeys & TVs spiking together on Black Friday). Analyzed "surprises" (actual - expected) to find true causal pairings.

- **Chronological 70/15/15 Split:** Rejected K-Fold cross-validation for time-series. **Trade-off:** Lower statistical power vs. **Airtight Data Hygiene**, preventing "Future Data Leakage" where the model accidentally learns from the future to predict the past.

- **Dimensionless Ratios for Anomalies:** Swapped raw dollars for ratios (Sales/Last_Year) in Isolation Forests. **Trade-off:** Extra feature engineering vs. **Eliminating Volume Bias**, ensuring massive stores aren't flagged as anomalies simply because they are large.

  

## Responsibilities That Go Beyond a Fresher

  

- **End-to-End Ownership:** Owned the entire lifecycle from defining business KPIs ($20.5M ROI target) to implementing the MLOps infra and deploying the final inference API on Kubernetes.

- **Strategic Tool Selection:** Justified the use of specific high-performance engines (DuckDB/Polars) based on financial constraints ($0 budget) and performance requirements (10-day timeline).

- **Defining Data Contracts:** Proactively established **Great Expectations** validations at the Silver/Gold boundaries to catch "silent failures" (e.g., cartesian joins or schema drift) before they reach the model.

- **Mathematical Leadership:** Identified and fixed architectural flaws in standard algorithms, such as Seasonality Bias in correlation matrices and Volume Bias in Isolation Forests.

- **Governance & Reproducibility:** Instituted strict DVC versioning and remote MLflow tracking, ensuring that every model prediction can be traced back to the exact code and dataset version used.

  

## Key Challenges and Solutions (Interview Focus)

  

### Challenge 1: The "Volume Outlier" Alert Fatigue

- **Situation**: Standard Isolation Forests flagged the largest Grocery departments as "anomalous" every week simply because their $150k sales were in the 99th percentile of company volume.

- **Task**: Detect true operational decoupling (e.g., store closures) without volume-based false positives.

- **Action**:

    - Engineered **dimensionless ratios** (Weekly Sales / Rolling 4-Week Average) in Polars.

    - Forced the algorithm to learn from **relative growth** rather than absolute dollars.

- **Result**: Successfully isolated a supply chain "Holiday Miss" where a store sold 10% less than its average during a peak week, which absolute volume metrics had missed.

  

### Challenge 2: Future Data Leakage in Time-Series

- **Situation**: Initial experimentation yielded an unrealistically perfect WMAPE (1%) because standard Scikit-Learn `train_test_split` was shuffling future data into the training set.

- **Task**: Establish a rigorous, leak-proof evaluation framework for a 476k row time-series dataset.

- **Action**:

    - Implemented a **3-way Chronological Split** (Train 70% / Val 15% / Test 15%).

    - Wrote custom PyTest assertions to verify that no date in the training set existed in the future of the test set.

- **Result**: Reduced perceived accuracy to a realistic 7.76%, providing executives with a reliable, "unseen" benchmark they could trust for financial forecasting.

  

### Challenge 3: "Fake" Correlations in Market Basket Analysis

- **Situation**: Raw correlation analysis showed a 0.95 relationship between Turkeys and Televisions, suggesting a cross-sell opportunity that was actually just a shared Black Friday spike.

- **Task**: Isolate true causal cross-department affinity for a $110M markdown budget.

- **Action**:

    - Calculated **Seasonal Residuals** (Actual Sales - Last Year's Sales) using DuckDB.

    - Performed Pearson Correlation strictly on these "surprises" to strip out predictable seasonality.

- **Result**: Discovered a true latent link between Camera discounts and subsequent Photo Printing volume, allowing Marketing to preserve gross margin on lead items.

  

## Likely Interview Questions (with Pointers)

  

### Architecture & Data Flow

- **Why use PySpark and Polars together?**

  *   *Pointer:* PySpark for horizontal scale/ingestion (Bronze); Polars for vertical speed/transformation (Silver). See `docs/02_architecture_tradeoffs.md`.

- **Explain your Medallion layers for this project.**

  *   *Pointer:* Bronze (Delta tables via Spark), Silver (Cleaned/Forward-filled via Polars), Gold (OBT ML-ready via DuckDB). See `docs/03_medallion_architecture.md`.

- **How did you handle the OBT storage vs. join overhead trade-off?**

  *   *Pointer:* Parquet dictionary encoding compresses repeating strings, making "One Big Table" more efficient than runtime joins. See `docs/02_architecture_tradeoffs.md`.

  

### Algorithms & Models

- **How did you optimize XGBoost for this specific data?**

  *   *Pointer:* Used **Optuna** for Bayesian search and `TransformedTargetRegressor` for log-transformed sales. See `docs/06_model_card.md`.

- **Why use Isolation Forest for anomaly detection?**

  *   *Pointer:* Unsupervised, handles multi-dimensional outliers, and efficient on tabular data. See `docs/10_anomaly_detection.md`.

- **How do you explain a 7.76% WMAPE to a non-technical manager?**

  *   *Pointer:* "On average, our predictions are within 7.7% of actual sales, allowing us to cut 9.5% of wasted safety stock." See `docs/01_business_context.md`.

  

### Reliability & Testing

- **How do you ensure data quality in your pipeline?**

  *   *Pointer:* **Great Expectations** contracts on Silver/Gold layers to check for nulls and schema drift. See `src/data/validate_gold.py`.

- **What is your strategy for monitoring model drift?**

  *   *Pointer:* Integrated **Evidently AI** to monitor distribution shifts in features like CPI and Fuel Prices. See `src/observability/drift_monitor.py`.

  

## Metrics & Numbers Reference

  

| Category | Metric | Value |

| :--- | :--- | :--- |

| **Business** | Annualized Revenue | $2.45 Billion |

| **Business** | Released Working Capital | **$20.53 Million** |

| **Business** | Manual Labor Saved | 320 hours / month |

| **Data** | Scale | 45 Stores / 99 Departments |

| **Data** | Granularity | 4,455 individual time series |

| **Data** | Volume | 476,000 distinct data points |

| **ML** | Baseline WMAPE (LYSW) | 11.85% |

| **ML** | Final WMAPE (XGBoost) | **7.76%** |

| **ML** | Accuracy Gain | 4.09% Absolute / 35% Relative |

| **Analytics** | Cluster Silhouette Score | 0.521 (K=3) |

| **Analytics** | Market Basket Clusters | Supercenter, Standard, Express |

| **Engineering**| Ingestion Lag | Native Delta/Parquet partitioning |

| **Infrastructure**| Budget | $0 (Open Source Stack) |

| **Timeline** | Pilot Delivery | 10 Days |