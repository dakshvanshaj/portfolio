Github: https://github.com/dakshvanshaj/flights-price-prediction-mlops
Docs: [Flight Prices](https://dakshvanshaj.com/flights-price-prediction-mlops/)

# Flights Price Prediction MLOps – Resume & Interview Cheat Sheet
June 2025 - Novemeber 2025
## Resume Bullets (Quantified Impact)
- **Engineered an end-to-end MLOps pipeline** using **DVC** and **GitHub Actions**, automating the lifecycle from ingestion to deployment on **Google Cloud Run**, reducing manual deployment overhead by 100%.
- **Architected a robust CI/CD workflow** that automates Docker image builds, pushes to **Google Artifact Registry**, and triggers blue-green deployments to **Cloud Run** upon versioned Git tag pushes (`v*`).
- **Deployed a centralized MLflow Tracking Server** on **AWS EC2**, utilizing **PostgreSQL on RDS** for metadata and **S3** for artifact storage, enabling team-wide experiment visibility and model reproducibility.
- **Optimized a LightGBM champion model** achieving a **Test Set RMSE of $7.60** and an **R² score of 0.99956**, significantly outperforming a Linear Regression baseline (RMSE $42.64).
- **Reduced model overfitting from ~92% to ~20%** by diagnosing and eliminating data leakage in engineered features through rigorous **SHAP** interpretability analysis and iterative experimentation.
- **Implemented a Medallion Architecture** (Bronze, Silver, Gold) using **Parquet** and **DVC**, ensuring data traceability and reducing storage footprints compared to raw CSV formats.
- **Integrated Great Expectations validation gates** at every pipeline stage (Bronze/Silver/Gold), ensuring 100% schema adherence and preventing "garbage-in, garbage-out" data corruption.
- **Developed a high-performance prediction API** using **FastAPI** and **Docker**, supporting real-time inference with an interactive **Streamlit** frontend for model explainability.
- **Managed secure credential injection** by integrating **Google Secret Manager** with Cloud Run, ensuring zero-exposure of sensitive API keys and database credentials in production environments.

## Skills and Tech Stack

### Languages & Core
- **Primary:** Python (3.12+), SQL.
- **Libraries:** pandas, NumPy, scikit-learn, LightGBM, XGBoost, SHAP, Great Expectations.

### Data & Storage
- **Formats:** Parquet (Silver/Gold layers), CSV (Raw).
- **Versioning:** DVC (Data Version Control) with S3-compatible remote storage.

### ML / Analytics
- **Evaluation Metrics:** RMSE ($7.60), MAE ($5.50), R² (0.99956).
- **Interpretability:** SHAP (Summary plots, Waterfall, Force plots, Dependence plots).
- **Tracking:** MLflow (Experiment tracking, Parameter logging, Model registry).

### Orchestration & Infrastructure
- **Cloud (GCP):** Cloud Run (Serverless serving), Artifact Registry, Secret Manager.
- **Cloud (AWS):** EC2 (Tracking server), RDS (PostgreSQL), S3 (Artifact store), Elastic IP (Static IP for tracking server).
- **CI/CD:** GitHub Actions (Automated linting, testing, deployment).
- **Containerization:** Docker (Multi-stage builds for prediction server).
- **Environment:** `uv` (Fast Python package installer and resolver).

### Tooling & Practices
- **Testing:** pytest (Unit, Integration, E2E), Mocking.
- **Documentation:** MkDocs, Mermaid.js.
- **Frontend:** Streamlit.
- **Backend:** FastAPI, Uvicorn.

## Design Decisions and Trade-offs
- **Medallion Architecture (Bronze/Silver/Gold):** Chose a layered approach to separate raw ingestion from feature engineering. Trade-off: Increased complexity vs. significantly improved data governance and easier debugging.
- **AWS for Tracking vs. Local:** Deployed MLflow on AWS EC2/RDS/S3 instead of local storage. Trade-off: Infrastructure cost/maintenance vs. centralized access, persistence, and team-wide collaboration.
- **Google Cloud Run for Serving:** Selected a serverless approach for the prediction API. Trade-off: Cold start latency vs. zero-maintenance scaling and significantly lower costs for fluctuating traffic.
- **Parquet vs. CSV:** Used Parquet for intermediate layers. Trade-off: Binary format vs. massive storage savings and faster I/O for columnar processing.
- **LightGBM over XGBoost:** Selected LightGBM as the champion due to superior stability (lower overfitting gap) and faster training times (~2.5 min) despite similar initial accuracy.
- **Removing 'Route' Feature:** Removed a highly predictive but "leaky" engineered feature. Trade-off: Lower initial accuracy vs. a model that generalizes to unseen dates and truly understands temporal patterns.

## Responsibilities That Go Beyond a Fresher
- **Architecting Cloud Infrastructure:** Designed and provisioned a multi-cloud environment using AWS for experiment tracking and GCP for model serving.
- **Implementing Secure CD Pipelines:** Built a production-grade CD pipeline that handles secure secret injection and versioned artifact management.
- **Proactive Bug Diagnosis:** Identified "too good to be true" metrics, suspected data leakage, and conducted a deep-dive SHAP analysis to prove and fix the overfitting issue.
- **Infrastructure as Code:** Defined the entire ML pipeline (DVC) and CI/CD (GitHub Actions) as code, ensuring the system is reproducible by any developer with one command (`dvc repro`).
- **Data Quality Advocacy:** Not just writing code, but setting up automated quality gates (Great Expectations) to handle data drift and schema changes proactively.

## Key Challenges and Solutions (Interview Focus)

### Challenge 1: Securely Serving Models in Production
- **Situation**: The prediction server needed access to models in a private S3 bucket and an MLflow server on a remote EC2 instance.
- **Task**: Implement a secure, containerized connection without exposing credentials.
- **Action**:
    - Containerized the API with **Docker** and used **Google Secret Manager** to inject credentials at runtime.
    - Configured **AWS IAM Roles** for the MLflow EC2 instance to grant least-privilege access to S3 without static keys.
- **Result**: Successfully deployed a secure, serverless API on Cloud Run that fetches the latest champion model dynamically from the model registry.

### Challenge 2: Severe Model Overfitting
- **Situation**: Initial LightGBM models showed near-perfect accuracy (CV RMSE ~$1.02), which was suspicious for a real-world flight dataset.
- **Task**: Determine if the model was truly accurate or benefiting from data leakage.
- **Action**:
    - Conducted a SHAP analysis which revealed a single engineered feature (`route`) was dominating 90% of the model's decisions.
    - Discovered the model was effectively "memorizing" prices for specific routes rather than learning generalizable patterns.
- **Result**: Removed the `route` feature and simplified preprocessing, stabilizing the model with a healthy CV-Test gap.

### Challenge 3: Pipeline Reproducibility & Environment Parity
- **Situation**: Discrepancies between local development and cloud deployment environments led to "it works on my machine" errors.
- **Task**: Ensure 100% environment parity and pipeline reproducibility.
- **Action**:
    - Switched to **`uv`** for lightning-fast, locked dependencies and used **Multi-stage Docker builds** to optimize image size.
    - Orchestrated the workflow with **DVC**, making data and code versions inseparable.
- **Result**: Reduced Docker image build times and ensured that every deployment is a bit-for-bit match of the validated local environment.

## Likely Interview Questions (with Pointers)

### Architecture & Data Flow
- **Explain your multi-cloud setup (AWS + GCP).** (Refers to documentation)
- **How do you handle secrets in Cloud Run?** (Refers to Secret Manager integration)
- **What is the benefit of using an Elastic IP for your MLflow server?** (Refers to Persistent tracking URI)

### Algorithms / Models / Business Logic
- **Why was LightGBM chosen over XGBoost?** (Refers to model selection report)
- **How did you handle temporal features?** (Refers to Cyclical feature engineering)

### Reliability, Testing, and Monitoring
- **How does your CD pipeline handle versioning?** (Refers to Git tag triggers)
- **How do you validate data quality in production?** (Refers to Great Expectations suites)

## Metrics & Numbers Reference

| Category | Metric | Value |
| :--- | :--- | :--- |
| **Model Performance** | Test Set RMSE | **$7.60** |
| | CV RMSE | **$9.57** |
| | R² Score | **0.99956** |
| **Cloud Infrastructure** | Deployment Platform | **Google Cloud Run** |
| | Artifact Storage | **AWS S3** |
| | Metadata Database | **AWS RDS (PostgreSQL)** |
| | Image Registry | **Google Artifact Registry** |
| **Efficiency** | Training Time (LGBM) | **~2.5 min** |
| | Overfitting Reduction | From **92%** gap to **20%** |
| **Testing** | Validation Gates | 3 (Bronze, Silver, Gold) |


