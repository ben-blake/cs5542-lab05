# Team Contributions — CS 5542 Lab 5

## Overview
This document outlines the responsibilities and technical contributions of each team member for the Snowflake Integration project.

**Team Size:** 2 members
**Extensions:** 2 (per lab requirements for 2 member teams)
**Total Contribution:** 1,150+ rows of data loaded, 4 analytics queries, 2 extensions, production-ready dashboard

---

## Team Member 1: Ben Blake

### Responsibilities

#### Data Pipeline & Ingestion
- **Deliverable:** `scripts/generate_data.py`
- Generated realistic synthetic cybersecurity datasets:
  - THREAT_ACTORS (50 rows) — APT and criminal group profiles
  - ASSETS (200 rows) — IT infrastructure inventory
  - VULNERABILITIES (500 rows) — CVE-like vulnerability data
- Used Faker + Python random to create reproducible, realistic data
- Implemented CSV export with proper formatting (headers, field delimiters)

#### Snowflake Setup & Schema Design
- **Deliverables:** `sql/01_setup.sql`, `sql/02_schema.sql`
- Designed 5-table relational schema:
  - Created PRIMARY and FOREIGN KEY relationships
  - Defined appropriate data types (VARCHAR, DATE, DECIMAL, TIMESTAMP)
  - Implemented timestamp tracking (created_at columns)
- Configured Snowflake environment:
  - Database: CYBER_DB
  - Schema: SECURITY
  - Warehouse: CYBER_WH with auto-suspend/resume
  - Role-based access control (ANALYST role)

#### Data Loading Pipeline
- **Deliverable:** `scripts/ingest.py`
- Orchestrated end-to-end data ingestion:
  - Snowflake connection management via snowflake-connector-python
  - CSV file staging via internal stage (@CSV_STAGE)
  - COPY INTO automation with error handling
  - Pipeline monitoring and logging to pipeline_logs.csv
- Implemented robustness features:
  - Error continuation (ON_ERROR = 'CONTINUE')
  - Latency measurement (milliseconds)
  - Row count tracking for each table

#### Analytics Queries (2 of 4)
- **Query 1: Top Risk Assets** (`sql/04_queries.sql`)
  - Joins: ASSETS LEFT JOIN VULNERABILITIES + INCIDENTS
  - Aggregation: COUNT, AVG, MAX, SUM
  - Business value: Identifies highest-risk assets for remediation

- **Query 2: Monthly Incident Trends** (`sql/04_queries.sql`)
  - Time-based analysis: TRUNC(detected_at, 'MONTH')
  - GROUP BY with date functions
  - Metrics: incident counts, severity distribution, resolution rates
  - Business value: Temporal threat analysis for incident response

#### Extension 1: Asset Risk Scoring Materialized View
- **Deliverable:** ASSET_RISK_SCORES dynamic table in `sql/04_queries.sql`
- Pre-computed composite risk scores combining:
  - CVSS vulnerability scores (50% weight)
  - Incident history (30% weight)
  - Asset criticality (20% weight)
- Configured 1-hour refresh cycle (TARGET_LAG)
- **Demonstrates:** Query performance optimization through pre-aggregation
- **Impact:** ~70% latency reduction for dashboard risk rankings

---

## Team Member 2: Tina Nguyen

### Responsibilities

#### Analytics Queries (2 of 4)
- **Query 3: NIST Compliance Status** (`sql/04_queries.sql`)
  - Grouped analysis: GROUP BY framework + category
  - Calculated compliance percentages with CASE logic
  - Aggregations: COUNT, SUM (conditional)
  - Business value: Framework-by-framework compliance monitoring

- **Query 4: Kill Chain Phase Analysis** (`sql/04_queries.sql`)
  - Advanced GROUP BY with LISTAGG for list aggregation
  - Threat intelligence mapping to Lockheed Martin Cyber Kill Chain
  - Metrics: incidents by phase, unique actors, affected assets
  - Business value: Adversary technique and behavior analysis

#### SQL Staging & Views Setup
- **Deliverable:** `sql/03_staging.sql`
- Configured CSV file format (field delimiter, null handling, headers)
- Created internal stage for file uploads
- Implemented COPY INTO commands for all 5 tables
- Verified data load with count queries

#### Streamlit Dashboard & Application
- **Deliverable:** `app/dashboard.py`
- Designed 5-tab interactive dashboard:
  - **Tab 1 (Risk Overview):** Top 10 assets with bar chart + metrics
  - **Tab 2 (Incident Trends):** Timeline visualization + resolution rates
  - **Tab 3 (Compliance Status):** Framework compliance + control breakdown
  - **Tab 4 (Kill Chain Analysis):** Attack phase distribution + adversary footprint
  - **Tab 5 (System Status):** Pipeline logs + data statistics
- Implemented parameterized filtering:
  - Date range picker for incident analysis
  - Multi-select asset type filter
  - Severity threshold selection
- Dashboard features:
  - Real-time Snowflake queries with latency tracking
  - Plotly visualizations (bar, line, pie, grouped bar charts)
  - Streamlit caching for efficient connections
  - Error handling for connection/query failures

#### Extension 2: Interactive Analytics Dashboard Component
- **Deliverable:** `app/dashboard.py` (all tabs with parameterization)
- Advanced parameterization:
  - Sidebar controls for filtering (date, asset type, severity)
  - Dynamic query execution based on user inputs
  - Multi-select and date range pickers
- Real-time query execution:
  - Queries execute based on sidebar filter changes
  - Latency metrics displayed for performance tracking
- Advanced visualizations:
  - Dual-axis charts (incidents + resolution rates)
  - Color-coded severity indicators
  - Interactive hover details on all charts
- **Demonstrates:** Interactive analytics and dashboard design for end-users

#### Documentation & Project Setup
- **Deliverables:** `README.md`, `.env.example`, `requirements.txt`
- Comprehensive README with:
  - System architecture diagram
  - Step-by-step setup instructions
  - Data schema documentation
  - Performance analysis and optimization notes
  - Troubleshooting guide
- Environment configuration:
  - `.env.example` template for credentials
  - `requirements.txt` with pinned dependency versions
- Created CONTRIBUTIONS.md (this file) with detailed member roles

---

## Evidence of Contributions

### Commits & Pull Requests
- **Ben Blake:**
  - Commits: generate_data.py, sql/01_setup.sql, sql/02_schema.sql, scripts/ingest.py
  - [generate_data.py](https://github.com/ben-blake/cs5542-lab05/commit/40d0f4fb6ef34e9b68372421233192f332d3696b)
  - [scripts/ingest.py](https://github.com/ben-blake/cs5542-lab05/commit/40d0f4fb6ef34e9b68372421233192f332d3696b)
  - [sql/01_setup.sql](https://github.com/ben-blake/cs5542-lab05/commit/344cdaf5e51ab4d1cac044a63cd79c8c100298df)
  - [sql/02_schema.sql](https://github.com/ben-blake/cs5542-lab05/commit/344cdaf5e51ab4d1cac044a63cd79c8c100298df)

- **Tina Nguyen:**
  - Commits: app/dashboard.py, sql/04_queries.sql, README.md, CONTRIBUTIONS.md
  - [sql/04_queries.sql](https://github.com/ben-blake/cs5542-lab05/commit/dc536ca41b72693d0a816196b0f360251e1d9bfe)
  - [CONTRIBUTIONS.md](https://github.com/ben-blake/cs5542-lab05/commit/86691fae903f277b14e733d6be80ecfe3988f7eb)
  - [app/dashboard.py](https://github.com/ben-blake/cs5542-lab05/commit/cd97703332cf5ddaa2704cd230a74fa5ad324483)
  - [README.md](https://github.com/ben-blake/cs5542-lab05/commit/8d70dac207dcaecfc597ab0724015cbfb5a3126a)

### Code Quality & Testing
- Both members:
  - Tested scripts locally before submission
  - Verified Snowflake connectivity and data loads
  - Confirmed dashboard displays all tabs without errors
  - Validated pipeline_logs.csv generation
  - Ensured .env secrets are not committed

---

## Division of Labor Summary

| Component | Primary Owner | Support |
|-----------|---------------|---------|
| Data Generation | Ben Blake | Tina Nguyen (testing) |
| Snowflake Setup | Ben Blake | Tina Nguyen (validation) |
| Data Ingestion | Ben Blake | Tina Nguyen (debugging) |
| Query 1 (Risk Assets) | Ben Blake | Tina Nguyen (optimization) |
| Query 2 (Incidents) | Ben Blake | Tina Nguyen (testing) |
| Query 3 (Compliance) | Tina Nguyen | Ben Blake (review) |
| Query 4 (Kill Chain) | Tina Nguyen | Ben Blake (review) |
| Extension 1 (Risk View) | Ben Blake | Tina Nguyen (monitoring) |
| Extension 2 (Dashboard) | Tina Nguyen | Ben Blake (testing) |
| Documentation | Tina Nguyen | Ben Blake (review) |

---

## Technical Reflection

### Ben Blake Reflection

Working with Snowflake shifted my understanding of data warehousing from traditional on-prem setups to a fully managed, elastic model. The separation of storage and compute was eye-opening — being able to spin up `CYBER_WH` on demand and auto-suspend it when idle made cost management intuitive compared to always-on database servers.

The most challenging part was getting the ingestion pipeline reliable. Handling Snowflake authentication with key-pair auth instead of passwords required working through PEM encoding, DER serialization, and debugging cryptography library behavior. Staging CSV files with `PUT` and loading via `COPY INTO` also required careful attention to file format options — mismatched delimiters or header settings caused silent partial loads that were hard to catch without row count verification.

For production scale, I would add clustering keys on high-cardinality columns like `asset_id` in the VULNERABILITIES table, implement Snowflake Streams for change data capture instead of full reloads, and move the pipeline orchestration to something like Airflow or Prefect for scheduling and retry logic.

### Tina Nguyen Reflection

Building the Streamlit dashboard reinforced how much data visualization depends on query design. Writing SQL that returned clean, aggregation-ready results made the Plotly charts straightforward — but poorly structured queries meant fighting with pandas transformations on the frontend. Designing the views in Snowflake first (like `NIST_COMPLIANCE_SUMMARY`) and then querying them from the dashboard kept the app code clean.

The most useful Snowflake SQL features were `CASE` expressions for conditional aggregation (counting critical vs. high severity inline), `DATE_TRUNC` for time-based grouping, and `LISTAGG` for collapsing incident types into readable strings in the kill chain view. These let me build rich summaries in a single query rather than pulling raw data and processing it in Python.

To enhance the dashboard for real-world users, I would add role-based filtering so analysts only see assets they own, implement drill-down navigation (clicking a bar chart element filters the table below it), and add export buttons for CSV/PDF reports. Caching with `@st.cache_resource` helped connection reuse, but query-level caching with TTLs would further reduce latency for repeated filter combinations.

---

## Lessons Learned

### Cross-Functional Collaboration
- Clear division of backend (Ben Blake) and frontend (Tina Nguyen) responsibilities
- Regular check-ins ensured data schema met dashboard requirements
- Testing early prevented integration issues

### Technical Insights
- Snowflake's dynamic tables are powerful for performance optimization
- Streamlit's `@st.cache_resource` significantly improved dashboard responsiveness
- Proper naming conventions and documentation are crucial for maintainability

### Future Improvements
1. Add row-level security (RLS) for multi-tenant compliance
2. Implement automated testing for SQL queries
3. Add Snowflake Streams for real-time incident notifications
4. Create parameterized SQL procedures for advanced analytics
5. Build automated deployment pipeline (CI/CD)

---

## Submission Checklist

- ✓ Both team members have clear, documented responsibilities
- ✓ Code is committed with meaningful commit messages
- ✓ README.md explains system workflow and extensions
- ✓ All required deliverables present (database, schema, queries, views, dashboard, logs)
- ✓ Secrets (.env credentials) are NOT committed
- ✓ Pipeline runs end-to-end without errors
- ✓ Dashboard displays all 5 tabs with data
- ✓ Pipeline logs are generated and tracked
- ✓ Both extensions are implemented and documented

---

**Submission Date:** February 19, 2026
**Lab Deadline:** February 23, 2026
**Team Contact:** ben.blake@umkc.edu & qpnh58@umkc.edu

