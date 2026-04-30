# COVID-19 Decision Intelligence & Risk Monitoring System
## How I automated national-level situational reporting, reducing data-to-decision time by 95%

**Project Type:** End-to-end Data Engineering & AI Automation  
**Goal:** Prove I can build robust data pipelines that feed directly into automated, LLM-driven executive reporting  
**Context:** Public health organizations face massive "action lag" where data takes days to become actionable insights.

<div style="text-align: center; margin: 2rem 0;">
  <a href="https://dakshvanshaj.com/covid19-decision-intelligence-system/" class="cta-button" style="background: transparent; border: 2px solid var(--md-accent-fg-color); color: var(--md-accent-fg-color) !important;">View Full Technical Breakdown →</a>
  <a href="https://github.com/dakshvanshaj/covid19-decision-intelligence-system" class="cta-button" style="margin-left: 1rem; background: transparent; border: 2px solid var(--md-accent-fg-color); color: var(--md-accent-fg-color) !important;">View Source Code on GitHub →</a>
</div>

---

## The Business Problem
Using fragmented public health data across 36 states, I addressed a critical data velocity bottleneck:

<div class="card-container">
  <div class="glass-card">
    <h3>The Pain Points</h3>
    <ul>
      <li><strong>Data Fragmentation:</strong> Raw data from multiple sources had mismatched state names and messy headers, requiring 6+ hours of manual processing.</li>
      <li><strong>Action Lag:</strong> Analysts took 1–2 days to convert raw SQL results into strategic briefings for leadership.</li>
      <li><strong>Alert Fatigue:</strong> Tracking 20+ separate KPIs made it impossible to prioritize resource allocation effectively.</li>
    </ul>
  </div>
  <div class="glass-card" style="border: 2px solid var(--md-accent-fg-color);">
    <h3>The Solution Impact</h3>
    <p>By building a Medallion architecture and integrating GenAI, I reduced reporting lag from <strong>2 days to under 60 seconds</strong> and decreased administrative monitoring time by <strong>80%</strong>.</p>
  </div>
</div>

---

## My Process (How I Approached It)
This project highlights how I build pipelines that prioritize business actionability:

<div class="process-step">
  <h3>1. Data Standardization (The Foundation)</h3>
  <p>Engineered an end-to-end Medallion architecture (Bronze-Silver-Gold) in PostgreSQL. Used robust SQL cleaning scripts (regex, fuzzy matching) to standardize 40,000+ records with zero data loss.</p>
</div>

<div class="process-step">
  <h3>2. Risk Signal Engineering</h3>
  <p>Instead of exposing 20+ confusing KPIs, I designed a weighted risk scoring algorithm (combining Case Fatality, Vaccination, and Positivity Rates) to provide a single, actionable "Risk Signal".</p>
</div>

<div class="process-step">
  <h3>3. AI-Driven Automation</h3>
  <p>Integrated Google Gemini/Gemma LLMs using n8n orchestration to automatically synthesize complex SQL signals into executive briefings.</p>
</div>

<div class="process-step">
  <h3>4. Human-in-the-Loop Feedback</h3>
  <p>Architected a feedback system using n8n webhooks, allowing stakeholders to tune alert sensitivity directly from their dashboards, reducing false positives.</p>
</div>

---

## Technical Highlights

### Why This Stack?
*   **PostgreSQL (Medallion):** Strict auditing required for public health data. Preserving raw states while exposing engineered features ensures integrity.
*   **n8n over Airflow:** Lower engineering complexity combined with high visual visibility for stakeholders to understand the workflow.
*   **Docker Containerization:** Reduced environment setup time by 90% (from 4 hours to <10 minutes) for rapid deployment.

<div class="card-container">
  <div class="glass-card">
    <h3>Key Engineering Decisions</h3>
    <ul>
      <li><strong>Idempotent Pipelines:</strong> Used `TRUNCATE ... CASCADE` inside SQLAlchemy transactions for safe, repeatable "one-click" data refreshes.</li>
      <li><strong>7-Day Moving Averages:</strong> Implemented DAX smoothing logic in Power BI to filter out administrative reporting noise (like weekend lags).</li>
      <li><strong>Tiered Prompt Engineering:</strong> Designed specific LLM prompts grounded in strict SQL contexts to prevent AI "hallucination."</li>
    </ul>
  </div>
</div>

---

## Results & Impact

<div class="card-container">
  <div class="glass-card" style="text-align: center;">
    <h2 style="color: var(--md-accent-fg-color);">&lt;30s</h2>
    <p>ETL Runtime (Down from 6 hrs)</p>
  </div>
  <div class="glass-card" style="text-align: center;">
    <h2 style="color: var(--md-accent-fg-color);">95%</h2>
    <p>Faster Decision Speed</p>
  </div>
  <div class="glass-card" style="text-align: center;">
    <h2 style="color: var(--md-accent-fg-color);">80%</h2>
    <p>Less Monitoring Time</p>
  </div>
</div>

### Operational Wins:
*   **Automated Briefings:** Strategy briefings generated in under 60 seconds of data ingestion.
*   **Actionable Dashboards:** High-performance Streamlit portal for real-time ETL orchestration and Power BI for national situational awareness.
*   **Zero-Friction Deployment:** Fully containerized stack for immediate local spin-up.

---

## What This Proves About My Process

*   **I Solve the Right Problem:** The issue wasn't a lack of data; it was the *speed* of insight. I focused on automation and synthesis.
*   **I Build for the End User:** Executives needed a single risk score, not raw data tables. I engineered the metrics to match their decision-making process.
*   **I Deploy Safely:** "Human-in-the-Loop" design ensures that AI doesn't make unchecked decisions.

---

<div style="text-align: center; margin: 4rem 0;">
  <a href="../../contact" class="cta-button" style="font-size: 1.25rem; padding: 1rem 3rem;">Apply This Process to Your Business →</a>
</div>
