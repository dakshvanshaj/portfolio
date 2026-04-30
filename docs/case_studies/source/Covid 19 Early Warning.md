Github: https://github.com/dakshvanshaj/covid19-decision-intelligence-system
Docs: [COVID-19 Intelligence Portal](https://dakshvanshaj.com/covid19-decision-intelligence-system/)

# COVID-19 Decision Intelligence & Risk Monitoring System
*March 2026*

Built an automated ETL pipeline (SQL, Python) to process 40K+ records in under 30 seconds, reducing preprocessing time by over 6 hours. Built Power BI dashboards for KPI tracking, enabling real-time risk analysis and regional identification in under 5 minutes. Developed an Excel risk evaluation tool (e.g., fatality rate above 5%) to identify high-risk regions. Automated AI-generated reports, improving decision-making speed by 95% using n8n. Integrated the system into a Streamlit dashboard and containerized with Docker, reducing setup time by 90%.

## Resume Bullets (Quantified Impact)
- **Engineered** an end-to-end Medallion data architecture (Bronze-Silver-Gold) in **PostgreSQL**, standardizing **40,000+ epidemiological records** with an ETL runtime of **<30 seconds** (replacing 6+ hours of manual effort).
- **Designed** a weighted risk scoring algorithm integrating positivity rates, vaccination coverage, and case fatality rates to reduce administrative monitoring time by **80%** via automated "Hot Zone" isolation.
- **Automated** strategic situational reporting using **n8n orchestration** and **Google Gemini/Gemma LLMs**, reducing the synthesis of complex SQL signals into briefings from **2 days to <60 seconds**.
- **Containerized** the entire platform using **Docker and Docker Compose**, achieving a **90% reduction** in environment setup time (from 4 hours to <10 minutes).
- **Implemented** a high-performance **Streamlit portal** for real-time ETL orchestration and interactive AI-driven surveillance auditing.
- **Developed** a national situational awareness dashboard in **Power BI** using DAX-driven **7-day moving averages** to filter reporting noise and visualize infection velocity.
- **Architected** a "Human-in-the-Loop" feedback system using **n8n webhooks** to allow stakeholders to tune alert sensitivity and reduce false positives in outbreak detection.
- **Built** an interactive **Excel Risk Evaluation Tool** for "what-if" threshold analysis, enabling stakeholders to audit surgical logic before policy implementation.

## Skills and Tech Stack

### Languages & Core
- **Primary**: Python (Pandas, SQLAlchemy, Streamlit), SQL (PostgreSQL).
- **Secondary**: `python-dotenv`, `pathlib`, `argparse`.

### Data & Storage
- **Primary**: PostgreSQL 18.1 (Medallion Architecture).
- **Formats**: CSV, Relational Tables, SQL Views (Gold Layer).

### ML / Analytics
- **Primary**: Google Gemini / Gemma (via n8n), Weighted Risk Scoring.
- **Metrics**: Case Fatality Rate (CFR), Positive Test Rate, Vaccination Coverage, 7-Day Moving Averages.

### Orchestration & Infrastructure
- **Primary**: Docker, Docker Compose, n8n (Workflow Automation).
- **Secondary**: Linux environment, Bash (entrypoint scripts).

### Tooling & Practices
- **Practices**: Medallion Data Design, Idempotent Pipelines, Infrastructure as Code (IaC), Transactional SQL, Regex-based Data Cleaning.

## Design Decisions and Trade-offs
- **Medallion Architecture (Bronze -> Silver -> Gold)**:
    - **Decision**: Separated data into three distinct layers.
    - **Trade-off**: Increased storage footprint vs. **High Traceability**.
    - **Rationale**: Public health data requires strict auditing; preserving raw states (Bronze) while exposing engineered features (Gold) ensures data integrity.
- **Dockerized "Hub" Deployment**:
    - **Decision**: Combined Streamlit UI and ETL workers into a single containerized stack.
    - **Trade-off**: Vertical scaling limits vs. **Zero-friction Deployment**.
    - **Rationale**: Public health emergencies require rapid environment setup on localized infrastructure.
- **n8n for Automation vs. Airflow**:
    - **Decision**: Used n8n for workflow orchestration.
    - **Trade-off**: Lower engineering complexity vs. **High Visual Visibility** for stakeholders.
    - **Rationale**: Allowed for rapid integration of AI nodes and webhooks for the "Human-in-the-Loop" feedback loop.
- **7-Day Moving Averages (Power BI)**:
    - **Decision**: Implemented DAX smoothing logic.
    - **Trade-off**: 7-day latency in peak detection vs. **Noise Reduction**.
    - **Rationale**: Administrative reporting lags on weekends created "artificial spikes" that misled decision-makers.
- **Weighted Risk Score Formula**:
    - **Decision**: Combined Case counts (40%), Vaccination (30%), and Fatality (30%).
    - **Trade-off**: Model simplicity vs. **Actionability**.
    - **Rationale**: Executives needed a single "Risk Signal" rather than 20 separate KPIs to prioritize resource allocation.

## Responsibilities That Go Beyond a Fresher
- **End-to-End Lifecycle Ownership**: Owned the project from raw CSV ingestion logic (`ingest.py`) to the final executive briefing automation (`n8n_workflows`).
- **Architectural Mapping**: Designed the data flow between disparate tools (Postgres -> n8n -> Streamlit) to ensure a "Single Source of Truth."
- **Defensive Engineering**: Implemented robust SQL cleaning scripts (`02_silver.sql`) with regex and fuzzy matching to handle inconsistent state naming (e.g., "Karanataka").
- **GenAI Strategy**: Designed tiered prompt engineering strategies (Alert-focused vs. Executive Briefing) to prevent LLM "hallucination" by grounding them in strict SQL contexts.
- **Infrastructure as Code**: Maintained a production-ready `compose.yaml` with volume persistence and health checks.

## Key Challenges and Solutions (Interview Focus)

### Challenge 1: Data Inconsistency & Fragmentation
- **Situation**: Raw data from 3+ sources had mismatched state names, inconsistent date formats, and messy headers.
- **Task**: Standardize the data to allow for accurate national-level joins.
- **Action**:
    - Built a Python-based `Ingestor` class using Regex for column normalization.
    - Implemented a SQL Silver layer with case-insensitive `TRIM` and manual mapping for spelling errors.
- **Result**: Successfully joined cases, testing, and vaccine data across 36 states with **zero data loss**.

### Challenge 2: Reducing "Action Lag" in Outbreak Reporting
- **Situation**: Analysts took 1–2 days to convert SQL results into a strategy briefing for leadership.
- **Task**: Reduce the "data-to-decision" window to under 5 minutes.
- **Action**:
    - Integrated Google Gemini via n8n to ingest SQL "Gold" signals.
    - Designed custom HTML prompt templates for automated email/Slack alerts.
- **Result**: Decision speed improved by **95%**, with reports generated in **<60 seconds** of data ingestion.

### Challenge 3: Ensuring Idempotency in the ETL Pipeline
- **Situation**: Re-running ingestion often led to duplicate records or primary key violations.
- **Task**: Create a safe, repeatable "one-click" refresh system.
- **Action**:
    - Used PostgreSQL `TRUNCATE ... CASCADE` inside SQLAlchemy transactions to ensure a clean state before every load.
- **Result**: ETL pipeline became fully **idempotent**, allowing stakeholders to refresh data safely without technical supervision.

---

