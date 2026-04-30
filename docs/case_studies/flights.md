# Flights Price Prediction MLOps Pipeline
## How I engineered a zero-overhead, multi-cloud MLOps deployment pipeline

**Project Type:** MLOps & CI/CD Engineering  
**Goal:** Prove I can architect secure, automated, and reproducible machine learning systems  
**Context:** Many ML models fail to reach production due to deployment friction, environment discrepancies, and lack of tracking.

<div style="text-align: center; margin: 2rem 0;">
  <a href="https://dakshvanshaj.com/flights-price-prediction-mlops/" class="cta-button" style="background: transparent; border: 2px solid var(--md-accent-fg-color); color: var(--md-accent-fg-color) !important;">View Full Technical Breakdown →</a>
  <a href="https://github.com/dakshvanshaj/flights-price-prediction-mlops" class="cta-button" style="margin-left: 1rem; background: transparent; border: 2px solid var(--md-accent-fg-color); color: var(--md-accent-fg-color) !important;">View Source Code on GitHub →</a>
</div>

---

## The Business Problem
Building an accurate model is only half the battle. This project tackled the operational challenges of deploying ML in the real world:

<div class="card-container">
  <div class="glass-card">
    <h3>The Pain Points</h3>
    <ul>
      <li><strong>Deployment Friction:</strong> Manual, error-prone processes for deploying models to production servers.</li>
      <li><strong>"It Works on My Machine":</strong> Environment discrepancies leading to inconsistent predictions and broken dependencies.</li>
      <li><strong>Zero Traceability:</strong> Difficulty tracking which data version produced which model, making rollbacks impossible.</li>
    </ul>
  </div>
  <div class="glass-card" style="border: 2px solid var(--md-accent-fg-color);">
    <h3>The Solution Impact</h3>
    <p>By automating the entire CI/CD pipeline and implementing centralized tracking, I achieved a <strong>100% reduction in manual deployment overhead</strong> while ensuring total model reproducibility.</p>
  </div>
</div>

---

## My Process (How I Approached It)
This project demonstrates how I bridge the gap between data science and reliable software engineering:

<div class="process-step">
  <h3>1. Centralized Experiment Tracking</h3>
  <p>Deployed an MLflow Tracking Server on AWS EC2 (backed by RDS and S3) to provide a single source of truth for all model metadata, parameters, and artifacts.</p>
</div>

<div class="process-step">
  <h3>2. Immutable Data Versioning</h3>
  <p>Implemented a Medallion Architecture (Bronze, Silver, Gold) using Parquet and DVC. This linked exact data versions to the code that processed them, guaranteeing reproducibility.</p>
</div>

<div class="process-step">
  <h3>3. Automated CI/CD Workflows</h3>
  <p>Architected GitHub Actions to automatically lint, test, build Docker images, and push to Google Artifact Registry upon versioned Git tag pushes.</p>
</div>

<div class="process-step">
  <h3>4. Secure Serverless Deployment</h3>
  <p>Configured Google Cloud Run for blue-green deployments, integrating with Google Secret Manager to inject credentials at runtime—ensuring zero exposure of sensitive keys.</p>
</div>

---

## Technical Highlights

### Why This Stack?
*   **Google Cloud Run:** Serverless serving means zero-maintenance scaling and lower costs for fluctuating API traffic.
*   **LightGBM:** Chosen as the champion model for its superior stability and rapid training times (~2.5 mins).
*   **Multi-Cloud Setup (AWS + GCP):** AWS utilized for cost-effective centralized tracking, while GCP handled high-performance serverless inference.

<div class="card-container">
  <div class="glass-card">
    <h3>Key Engineering Decisions</h3>
    <ul>
      <li><strong>Validation Gates:</strong> Integrated Great Expectations at every pipeline stage to enforce strict schema adherence and prevent data corruption.</li>
      <li><strong>Parquet vs. CSV:</strong> Switched to Parquet for intermediate layers, achieving massive storage savings and faster columnar I/O.</li>
      <li><strong>Strict Environment Parity:</strong> Leveraged `uv` for lightning-fast dependency resolution and multi-stage Docker builds to ensure identical local and production environments.</li>
    </ul>
  </div>
</div>

---

## Results & Impact

<div class="card-container">
  <div class="glass-card" style="text-align: center;">
    <h2 style="color: var(--md-accent-fg-color);">$7.60</h2>
    <p>Test Set RMSE (LightGBM)</p>
  </div>
  <div class="glass-card" style="text-align: center;">
    <h2 style="color: var(--md-accent-fg-color);">100%</h2>
    <p>Manual Deployment Reduction</p>
  </div>
  <div class="glass-card" style="text-align: center;">
    <h2 style="color: var(--md-accent-fg-color);">0.999</h2>
    <p>R² Score Reliability</p>
  </div>
</div>

### Operational Wins:
*   **Zero-Downtime Deployments:** Automated blue-green deployments triggered entirely via Git tags.
*   **High-Performance API:** Built a fast prediction API using FastAPI, complemented by an interactive Streamlit frontend for explainability.
*   **Infrastructure as Code:** The entire ML pipeline can be reproduced by any developer using a single command (`dvc repro`).

---

## What This Proves About My Process

*   **I Build Systems, Not Just Models:** I understand that an ML model is useless if it can't be safely deployed and monitored.
*   **I Prioritize Security:** Integrating Secret Manager and AWS IAM Roles proves I follow production-grade security practices.
*   **I Engineer for Reliability:** Automated testing, validation gates, and strict version control are non-negotiable in my workflow.

---

<div style="text-align: center; margin: 4rem 0;">
  <a href="../../contact" class="cta-button" style="font-size: 1.25rem; padding: 1rem 3rem;">Apply This Process to Your Business →</a>
</div>
