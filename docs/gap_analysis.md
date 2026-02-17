# Gap Analysis — U.S. Airline Operational Performance (2023–2025)

*3NF warehouse and STTM are completed; they are not listed as gaps. This document focuses on the gap between current deliverables and full project objectives.*

---

## Part 1: Project Analysis

### 1.1 Positioning and Objectives

- **Course context**: MSDS 420 Database Systems—focus on database design, modeling, and analytical querying, not application development.
- **Business objectives**: Use BTS and related public data to analyze U.S. airline on-time performance, cancellations, and operational efficiency (2023–2025), supporting questions on controllable vs uncontrollable delays, patterns by airport/season/time-of-day, and links to complaints and financials.
- **Technical objectives**: Execute the full lifecycle from raw data → Staging → 3NF warehouse → star-schema Marts → analytical queries, with clear normalization, dimensional modeling, and source-to-target mapping.

### 1.2 Scope and Boundaries

- **Time**: README states 2022–2024; data and code use 2023–2025—documentation should be aligned.
- **Data**: BTS On-Time as primary fact source; airport reference, financials, and airport traffic as supporting; weather, jet fuel, and holidays as context; complaint data is referenced in README but not present in the repo.
- **Out of scope**: No automation pipelines, no app development; emphasis on modeling and SQL-based analysis.

### 1.3 Mapping to Business Questions

| Business question | Data required | Current support |
|-------------------|---------------|-----------------|
| Main causes of delays/cancellations | On-Time (delay causes + cancellation code) | Data available; needs loading and modeling |
| Controllable vs uncontrollable delays | On-Time (Carrier, NAS, Weather, Security, LateAircraft) | Fields present; model should make the split explicit |
| Patterns by airport, season, time of day | On-Time + time and airport dimensions | Requires dimensional model and time hierarchy |
| Relation to complaints and financials | Complaint data + financials + On-Time | Complaints missing; need documented join keys (carrier, time) between financials and On-Time |

---

## Part 2: Data Classification Analysis (Non-EDA)

Structural classification of data by type and grain; no exploratory statistics or charts.

### 2.1 By Data Nature

| Category | Source | Grain | Role in project |
|----------|--------|-------|-----------------|
| **Transactional / fact** | BTS On-Time | One row per flight | Core fact table; drives delay cause, controllability, and spatio-temporal analysis. |
| **Master / reference** | Airport Reference | One row per airport | Dimension attributes for airports; used for joins and slicing. |
| **Aggregate / reporting** | Financial Form 41, Airport Traffic | Carrier–quarter; airport–month | Supports “performance vs financials” and airport load vs delays. |
| **Derived** | Airport Traffic | Airport–year–quarter–month | Derived from On-Time; can be a separate mart or dimension supplement. |
| **Contextual** | Weather, jet fuel, holidays | Date or airport–date; month; date | Explains delays; not primary fact grain. |

### 2.2 By Source System and Update Frequency

- **BTS TranStats**: On-Time, airport reference, financials—authoritative, monthly/quarterly.
- **EIA**: Jet fuel—monthly; cost context.
- **OPM**: Federal holidays—annual.
- **Weather**: Daily; currently KJFK only.

### 2.3 Grain and Join Keys

- **On-Time**: Links to airports via ORIGIN/DEST, to carrier via OP_UNIQUE_CARRIER/OP_CARRIER_AIRLINE_ID.
- **Financials**: (AirlineID/UNIQUE_CARRIER, Year, Quarter); join to On-Time on carrier and time.
- **Airport traffic**: (Airport_Code, Year, Quarter, Month); join on airport and time.
- **Holidays / jet fuel**: Attach to time dimension by date or month.

---

## Part 3: Gap Analysis (Deep Dive)

This section compares **what the project has delivered so far** with **what the stated project objectives require**, and details each gap in terms of impact, target state, and actionable closure.

### 3.0 Current Deliverables vs. Project Objectives

| Project objective (from README) | Current state | Gap summary |
|---------------------------------|---------------|-------------|
| **Staging Layer** (raw + cleaned) | Only Python notebooks that merge/fix CSVs in `data/`; no staging DB tables or load scripts in `staging/` or `etl/`. | Staging exists as file-based prep only; not as a database layer with defined schema and load process. |
| **3NF Data Warehouse** | Assumed completed (per team). | — |
| **Data Marts (star schema)** | `marts/` is empty (.gitkeep only). No fact/dimension DDL, no load from DW to marts. | Marts are not implemented; slice-and-dice and hierarchies are not yet possible via SQL. |
| **STTM** | Assumed completed. | — |
| **Answer business questions with SQL** | No analytical queries in `sql/`; answers cannot be demonstrated end-to-end. | Business questions are not yet answerable from the repository’s database layer. |
| **EDA and documentation** | `eda/` empty; `docs/data_download_guide.md` and `docs/data_links.md` missing. | Reproducibility and data discovery are underdocumented. |

**Net effect**: The project has **data preparation (file-level)** and (by assumption) **3NF + STTM**, but lacks **Staging as a DB layer**, **Marts**, **analytical SQL**, and **supporting docs/EDA**. So there is a clear gap between “design and mapping done” and “full lifecycle and business-question support done.”

---

### 3.1 Data Gaps

Each row below states the **project goal** for data, the **current vs. target state**, **impact** on objectives, and **concrete actions** to close the gap.

---

#### D1. Time range and documentation alignment

| Aspect | Detail |
|--------|--------|
| **Project goal** | A single, clearly stated analysis period so that data, code, and docs are consistent. |
| **Current state** | README says “2022–2024”; data and preprocessing cover 2023–2025. New users and graders get conflicting signals. |
| **Target state** | README, data readmes, and any summary docs all state the same period (e.g. 2023–2025) and, if 2022 is excluded, briefly explain why. |
| **Impact** | Misalignment undermines reproducibility and scope clarity; can cast doubt on other documentation. |
| **Recommendation** | Update README (and any other scope statements) to “2023–2025.” If 2022 is never used, remove “2022” from all docs. Add one sentence on chosen scope (e.g. “Analysis period: 2023–2025 to match data availability.”). |

---

#### D2. Complaint data (fifth data source)

| Aspect | Detail |
|--------|--------|
| **Project goal** | Support the business question: “How does operational performance relate to customer complaints and financial trends?” Complaints are listed as source #4 in README. |
| **Current state** | No complaint dataset in the repo; no download instructions; no table or mart design for complaints. The question cannot be answered with current assets. |
| **Target state** | Either (a) add complaint data (file + download doc + STTM + schema), or (b) formally drop complaints from scope and restate the business question (e.g. “relation to financial trends” only) so that deliverables and questions match. |
| **Impact** | As long as README promises complaint-based analysis without data or schema, the project appears incomplete. Either deliver the capability or explicitly narrow the question. |
| **Recommendation** | Decide promptly: include DOT complaint data (and add source + doc + mapping) or remove “customer complaints” from the listed business question and from the data sources list. Document the decision in README or in `docs/scope.md`. |

---

#### D3. Data source list and LFS

| Aspect | Detail |
|--------|--------|
| **Project goal** | All used sources listed and obtainable; large files usable via LFS so that “download and run” is possible. |
| **Current state** | Jet fuel (EIA) and US holidays (OPM) are used but not in README “Data Sources.” README points to `docs/data_download_guide.md` and `docs/data_links.md`, which do not exist. Merged On-Time, airport reference, and weather are under LFS with no instructions. |
| **Target state** | README (or a single doc) lists every source (BTS, EIA, OPM, weather, and optionally complaints) with purpose. A short download guide exists and includes LFS: “run `git lfs install` and `git lfs pull` before use” and, where applicable, manual download steps for BTS/EIA/OPM. |
| **Impact** | Without this, the project cannot be reproduced by someone cloning the repo; grading and reuse are hindered. |
| **Recommendation** | Add `docs/data_links.md` (and optionally `docs/data_download_guide.md`) with all links and LFS instructions. Update README “Data Sources” to include EIA and OPM. In README or the guide, add a “Getting the data” subsection that mentions LFS and points to the doc. |

---

#### D4. Weather scope and naming

| Aspect | Detail |
|--------|--------|
| **Project goal** | Contextual weather for interpreting delays; docs that set correct expectations (single vs multi-airport). |
| **Current state** | Only KJFK weather; filename has a space (“KJFK _Weather_...”); README mentions “10–15 major airports” and “2022–2024,” which do not match the repo. |
| **Target state** | Filename normalized (no space). Docs state clearly: “Weather: currently KJFK only; period X–Y.” If multi-airport is out of scope, say so; if planned, add a one-line “Future: extend to top N airports.” |
| **Impact** | Prevents confusion and overpromising; keeps analysis conclusions (e.g. weather impact) scoped to what the data actually supports. |
| **Recommendation** | Rename the weather file (e.g. `KJFK_Weather_2023-02-01_to_2025-10-01.csv`). In README or data README, replace “10–15 airports” and “2022–2024” with the actual scope and note any extension plan. |

---

#### D5. Financial merge schema and On-Time 2024 fix

| Aspect | Detail |
|--------|--------|
| **Project goal** | Clean, consistent schemas for loading into DW/Marts; no duplicate or debug-only columns in “canonical” merged files; transformations documented for traceability. |
| **Current state** | Financial merged file has both `Year` and `YEAR` and a `source_file` column. On-Time 2024 had 43 vs 42 columns; the fix is in a notebook but not described in data docs or STTM. |
| **Target state** | Single year column in financial merge; `source_file` either removed or declared as “audit-only” in STTM. 2024 On-Time column fix (which column was dropped/merged and why) recorded in data docs or STTM. |
| **Impact** | Reduces load errors and ambiguity when building DW/Marts; supports audit and grading. |
| **Recommendation** | In the financial merge script: keep one year column, drop or document `source_file`. In STTM or `data/01_OnTime_Performance/README.md`, add a short “2024 column fix” note. |

---

#### D6. Data quality and types

| Aspect | Detail |
|--------|--------|
| **Project goal** | Known null rates and type issues so that ETL and analytics can handle them and so that results are interpretable. |
| **Current state** | Financial tables have many nulls (e.g. passenger revenue); On-Time has mixed-type columns (e.g. numeric IDs as text). No data-quality summary or EDA in the repo. |
| **Target state** | Brief data-quality view: which key fields have nulls, which columns are mixed type, and how they are handled (e.g. “excluded from financial analysis” or “cast to int in Staging 2”). EDA or a short doc is the natural place. |
| **Impact** | Without this, downstream model and query design are blind to data limitations; “performance vs financials” may be misleading if nulls are not acknowledged. |
| **Recommendation** | Add one EDA notebook or `docs/data_quality.md` that reports null counts and mixed-type columns for main tables and states the chosen handling. Reference it from README or STTM. |

---

### 3.2 Model Gaps

These gaps prevent the project from **delivering queryable Marts and from answering business questions via the intended architecture** (Staging → DW → Marts).

---

#### M1. Staging as a database layer

| Aspect | Detail |
|--------|--------|
| **Project goal** | README specifies “Staging 1: raw data loaded as-is” and “Staging 2: cleaned and standardized.” That implies tables (or equivalent) and a defined load process. |
| **Current state** | Only file-based merging and cleaning in notebooks; `staging/` and `etl/` are empty. There is no staging schema (e.g. MySQL tables) and no script that loads CSVs into those tables. |
| **Target state** | Staging 1 and Staging 2 represented by schemas (DDL) and at least one load path (e.g. Python or SQL) that populates them from the prepared CSVs. Documented in README or `docs/staging.md`. |
| **Impact** | Without staging as a DB layer, the “full lifecycle” is incomplete and the step from “files” to “warehouse” is not demonstrated. |
| **Recommendation** | Add DDL for Staging 1 (and optionally Staging 2) under `sql/` and a repeatable load script in `etl/` that reads from `data/` (or merged outputs) and loads into MySQL. Reference from README. |

---

#### M2. Data Marts (star schema)

| Aspect | Detail |
|--------|--------|
| **Project goal** | “Data Marts (dimensional model)—star schema with fact and dimension tables; supports slice-and-dice and hierarchies; slowly changing dimensions where needed.” |
| **Current state** | `marts/` is empty. No fact/dimension DDL, no load from DW to marts, no ability to run analytical queries against a star schema. |
| **Target state** | At least one star schema implemented: fact table(s) (e.g. flight delay/cancellation facts) and dimensions (time, airport, carrier, delay/cancel type). DDL in `sql/`; load from DW documented or scripted. Hierarchies (e.g. time: year → quarter → month → day) and SCD approach stated. |
| **Impact** | Marts are the main vehicle for “analytical querying” and “slice-and-dice” in the README. Without them, the project cannot show dimensional modeling or answer business questions in the intended way. |
| **Recommendation** | Design one star schema (e.g. flight delays/cancellations + time, airport, carrier, delay type). Add DDL; add a load script or clear instructions to populate marts from DW. Document hierarchies and any SCD in `docs/marts.md` or README. |

---

#### M3. Controllable vs uncontrollable delays in the model

| Aspect | Detail |
|--------|--------|
| **Project goal** | Business question: “Which delays are operationally controllable versus uncontrollable?” That requires the model to expose this split (e.g. Carrier + LateAircraft = controllable; Weather + NAS + Security = uncontrollable). |
| **Current state** | Raw On-Time has the five delay-cause columns but no explicit “controllable vs uncontrollable” attribute or dimension. If Marts are not yet built, there is no dimension or measure that encodes this. |
| **Target state** | In the mart (or DW), delays are classifiable as controllable vs uncontrollable—e.g. via a dimension attribute or a derived measure—so that a single query can answer “share of delay minutes by controllable vs uncontrollable.” |
| **Impact** | Without this, one of the four stated business questions cannot be answered clearly from the model. |
| **Recommendation** | In the star schema (or DW), add a delay-type dimension or fact attribute that maps the five causes to “controllable” vs “uncontrollable” (and optionally “unknown”). Document the mapping in STTM or `docs/marts.md`. Provide one example SQL that aggregates by this split. |

---

#### M4. Time (and optional airport) hierarchy

| Aspect | Detail |
|--------|--------|
| **Project goal** | “How do delay patterns vary by airport, season, and time of day?” Requires a time dimension with at least year, quarter, month, day, and ideally time-of-day band; optionally airport hierarchy (e.g. city, state). |
| **Current state** | No Marts yet, so no formal time or airport dimension with defined hierarchy. Raw data has date and time fields but no dimension table that supports roll-up. |
| **Target state** | Time dimension with levels (e.g. year → quarter → month → day; optionally hour or time band). Airport dimension with at least airport → city/state if useful for “by airport” and “by region.” |
| **Impact** | Enables “by season” and “by time of day” and “by airport” in one place; without it, the third business question is harder to answer in a consistent way. |
| **Recommendation** | In the mart DDL, define a time dimension table with hierarchy columns and an airport dimension (with city, state). In docs, state how “season” and “time of day” are derived (e.g. quarter for season; hour band for time of day). |

---

#### M5. ER or logical model diagrams

| Aspect | Detail |
|--------|--------|
| **Project goal** | README: “models/ — ER diagrams and data models.” Communicates design to graders and teammates. |
| **Current state** | `models/` contains only data-preprocessing notebooks; no ER or logical model for 3NF or star schema. |
| **Target state** | At least one diagram for the 3NF warehouse and one for the star schema (e.g. in `models/` as image or draw.io/mermaid), with a short note on main entities and relationships. |
| **Impact** | Improves clarity and demonstrates design thinking; aligns with README. |
| **Recommendation** | Add 3NF and star-schema diagrams under `models/` (e.g. `models/er_dw.png`, `models/star_marts.png`) and a brief `models/README.md` describing entities and relationships. |

---

### 3.3 Technical Optimization Gaps

These affect **reproducibility, runnability, and maintainability** of the project.

---

#### T1. Paths and portability

| Aspect | Detail |
|--------|--------|
| **Project goal** | Any teammate or grader can run preprocessing and ETL from a clone of the repo without editing paths. |
| **Current state** | Preprocessing notebooks use Windows absolute paths and machine-specific paths (e.g. `C:\Users\...`). |
| **Target state** | All paths are relative to project root (or an env var like `PROJECT_ROOT`); README states “run from repo root.” |
| **Impact** | Without portable paths, the project cannot be run as-is on another machine; undermines “demonstrate full lifecycle.” |
| **Recommendation** | Refactor notebooks to use `Path(__file__).resolve().parent` or `Path(".").resolve()` and paths relative to repo root. Add one sentence in README: “Run all notebooks and scripts from the project root directory.” |

---

#### T2. Environment and dependencies

| Aspect | Detail |
|--------|--------|
| **Project goal** | Clear Python and MySQL expectations so that others can replicate the environment. |
| **Current state** | No `requirements.txt` or `environment.yml`; no stated Python or MySQL version. |
| **Target state** | A dependency file (e.g. `requirements.txt` with pandas and any DB driver) and in README a line such as “Python 3.x; MySQL 8.x (or compatible).” |
| **Impact** | Reduces “works on my machine” issues and supports consistent grading. |
| **Recommendation** | Add `requirements.txt` (and optionally `environment.yml`). In README “Tools and Technologies,” add “Python 3.x”, “MySQL 8.x (or compatible),” and “See requirements.txt for Python packages.” |

---

#### T3. ETL and SQL content

| Aspect | Detail |
|--------|--------|
| **Project goal** | README: “etl/ — data loading and transformation scripts”; “sql/ — DDL, queries, and validation scripts.” Lifecycle is demonstrated by runnable scripts and queries that support business questions. |
| **Current state** | `etl/` and `sql/` contain only .gitkeep. No load scripts, no DDL (beyond what may exist for 3NF), no analytical queries. |
| **Target state** | `sql/`: DDL for staging, DW, and marts (if not elsewhere); at least 2–3 analytical queries that directly answer the business questions (or one per question). `etl/`: at least one repeatable load script (e.g. CSV → staging or DW → marts). |
| **Impact** | Without these, the repo does not show “analytical querying” or “data loading and transformation” in the intended structure. |
| **Recommendation** | Add staging (and marts) DDL to `sql/`; add 2–3 SQL files that implement the four business questions (or a single `sql/business_queries.sql` with commented sections). Add one ETL script (e.g. `etl/load_staging.py` or SQL) that can be run after data and LFS are in place. |

---

#### T4. EDA and run order

| Aspect | Detail |
|--------|--------|
| **Project goal** | README: “eda/ — exploratory data analysis notebooks.” Also, a clear sequence so that someone can “get data → prep → load → query” without guessing. |
| **Current state** | `eda/` is empty. No document that says “1) LFS pull, 2) download data (see docs), 3) run notebooks in models/data_preprocessing, 4) run ETL, 5) run SQL queries.” |
| **Target state** | At least one EDA notebook (e.g. summary stats, delay/cancel rates, nulls, one plot per main table). A short run-order note in README or `docs/run_order.md`. |
| **Impact** | EDA supports data quality (see D6) and shows engagement with the data; run order supports reproducibility. |
| **Recommendation** | Add one notebook in `eda/` (overview of On-Time and financials; delay/cancel by month or carrier). In README or `docs/run_order.md`, list steps: LFS pull → data download → preprocessing → ETL → run SQL. |

---

#### T5. Data quality checks and large-table strategy

| Aspect | Detail |
|--------|--------|
| **Project goal** | Basic validation so that load and analysis are trustworthy; a feasible strategy for ~20M+ On-Time rows (no silent timeouts or OOM). |
| **Current state** | No documented checks (nulls, uniqueness, referential integrity). No stated approach for loading very large CSVs. |
| **Target state** | ETL or a small script that checks key fields (e.g. flight key unique, required FKs present) and logs or documents results. Loading strategy documented (e.g. “load by year” or “Staging 1 by month”) so that others know how to run it. |
| **Impact** | Reduces risk of wrong results and failed runs during demos or grading. |
| **Recommendation** | Add simple checks in the staging/DW load script (e.g. row counts, sample uniqueness). In `docs/` or README, add a “Data loading” note: “On-Time is loaded in batches by year (or month); see etl/ for details.” |

---

### 3.4 Gap-to-Objectives Map

| Project objective | Gaps that block or weaken it | If closed |
|-------------------|------------------------------|-----------|
| Staging layer (raw + cleaned) | M1, T1, T3 | Staging schema and load script; portable paths; ETL in place. |
| Data Marts (star schema) | M2, M3, M4, M5, T3 | Marts DDL and load; controllable vs uncontrollable; time/airport hierarchy; diagrams. |
| Answer all four business questions with SQL | D2, M2, M3, M4, T3 | Complaint scope decided; marts and dimensions in place; example queries for each question. |
| Reproducibility and documentation | D1, D3, D4, T2, T4 | Aligned scope and sources; LFS and download docs; run order; environment doc. |
| Data and model quality | D5, D6, T5 | Clean merge schema; 2024 fix documented; EDA/data-quality note; basic validation and load strategy. |

---

### 3.5 Priority Summary

- **High (required to meet core objectives)**  
  - Align README with 2023–2025 and data sources (D1, D3).  
  - Resolve complaint scope and business question wording (D2).  
  - Implement Staging as a DB layer and at least one Mart with DDL and load (M1, M2).  
  - Add analytical SQL that answers the business questions (T3).  
  - Document run order and LFS (T4, D3); use relative paths (T1).  

- **Medium (strongly recommended)**  
  - Model controllable vs uncontrollable and time/airport hierarchy (M3, M4).  
  - Add EDA and data-quality documentation (D6, T4).  
  - Add ER/star diagrams (M5); environment and dependencies (T2).  
  - Document 2024 On-Time fix and financial merge schema (D5).  

- **Low (polish)**  
  - Weather filename and scope doc (D4).  
  - Data-quality checks and large-table load strategy in docs (T5).  
  - Additional example queries and multi-airport weather if in scope.  
