<div align="center">

# 🏥 Healthcare Patient Intelligence & Hospital Performance Analytics

**An end-to-end analytics pipeline that turns 55K+ hospital encounters into executive-ready intelligence — raw data → cleaned data model → MySQL → an 8-page Power BI decision-support dashboard.**

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?logo=mysql&logoColor=white)](https://www.mysql.com/)
[![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-ORM-D71F00)](https://www.sqlalchemy.org/)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![License](https://img.shields.io/badge/License-MIT-green)](#-license)

</div>

---

## 📖 Table of Contents

1. [Overview](#-overview)
2. [At a Glance](#-at-a-glance)
3. [Dashboard Preview](#-dashboard-preview)
4. [Key Findings & Business Recommendations](#-key-findings--business-recommendations)
5. [Project Architecture](#-project-architecture)
6. [Repository Structure](#-repository-structure)
7. [Dataset Schema](#-dataset-schema)
8. [Methodology](#-methodology)
9. [Getting Started](#-getting-started)
10. [Data Quality & Limitations](#-data-quality--limitations)
11. [Security](#-security)
12. [Tech Stack](#-tech-stack)
13. [Roadmap](#-roadmap)
14. [Contributing](#-contributing)
15. [License](#-license)
16. [Author](#-author)

---

## 📌 Overview

Hospitals sit on top of enormous volumes of admissions, billing, and clinical data — but that data rarely gets translated into decisions a CFO or Chief Medical Officer can act on. This project builds a complete analytics workflow that closes that gap:

- **Ingest & clean** 55K+ patient encounter records
- **Explore** demographic, clinical, and financial patterns programmatically (Python EDA)
- **Persist** the curated dataset to MySQL through a repeatable SQLAlchemy ETL pipeline
- **Visualize** the results in an 8-page, cross-filterable Power BI dashboard
- **Synthesize** the analysis into a prioritized, executive-facing action plan

The output isn't just charts. The dashboard's final page converts every KPI into a ranked business priority — signal, evidence, impact, recommended action — the same framework used in real hospital-operations steering committees.

---

## ⚡ At a Glance

- Built a **4-stage ETL + BI pipeline** (raw → clean → MySQL → Power BI) processing **55,000+ records** across 15+ dimensions
- Engineered derived features (`length_of_stay`, `age_group`) to unlock operational analysis unavailable in the raw extract
- Designed **8 interconnected dashboard pages** with cross-page slicers spanning patient demographics, clinical outcomes, hospital performance, operations, and billing
- Surfaced a **primary capacity-risk finding**: long-stay encounters (29.8% of volume) consume **50.1% of total bed-days** — a 2x over-representation
- Ruled out payer and clinical-cost concentration risk through evidence-based analysis, preventing wasted mitigation effort
- Delivered findings as a **ranked, four-priority executive action plan** rather than raw metrics, mirroring real operations-review deliverables

---

## 🖼️ Dashboard Preview

The Power BI report ships with 8 pages, each purpose-built for a different stakeholder question:

| # | Page | Answers |
|---|---|---|
| 1 | **Executive Healthcare Overview** | What's the top-line volume, billing, and growth story? |
| 2 | **Patient Intelligence** | Who are our patients — age, gender, blood type distribution? |
| 3 | **Medical Condition Intelligence** | Which conditions drive volume, cost, and abnormal-test rates? |
| 4 | **Hospital Performance Intelligence** | How do facilities compare on billing and length of stay? |
| 5 | **Admission & Operations Intelligence** | What's our admission mix and capacity utilization? |
| 6 | **Billing & Insurance Intelligence** | How is revenue distributed across payers and conditions? |
| 7 | **Treatment & Test Intelligence** | Do medications or test results correlate with cost or stay? |
| 8 | **Executive Recommendations** | So what? — the ranked action plan for leadership |

Full-page screenshots are available in [`/Screenshot`](Screenshot) for a visual walkthrough without opening Power BI Desktop.

---

## 📊 Key Findings & Business Recommendations

The dashboard's final page converts every metric into a ranked, four-priority action plan:

| Priority | Signal | Evidence | Impact | Recommended Action |
|---|---|---|---|---|
| 🔴 **P1 — Bed-Day Concentration** | Long-stay encounters (>21 days) are 29.8% of volume but consume **50.1% of bed-days** | Long-stay bed-day share is exactly double the encounter share | Structural over-consumption of capacity, not a one-off spike | Launch a dedicated LOS-reduction KPI targeting the >21-day segment; prioritize discharge planning for this cohort |
| 🟡 **P2 — Financial Concentration** | Top payer (Cigna, 20.3%) and top condition (Diabetes, 16.8%) shares are both within 1–2pp of a perfectly even split | No single payer or condition dominates revenue | **Reassuring finding** — no concentration risk | Deprioritize monitoring here; redirect analyst effort to Priority 1 |
| 🌸 **P3 — Long-Stay Pressure** | Long-stay % YoY is flat (▲0.2%) | LOS is uniformly distributed 1–30 days (~16.6% per 5-day band) with no natural tail cutoff | Structural pattern, not a temporary spike — won't self-correct | Track Long-Stay Bed-Day Share % as a standing operational KPI |
| 🔵 **P4 — Test / Condition Pattern** | Abnormal test rate (33.5%) is statistically tied with Normal (33.3%) and Inconclusive (33.1%) | Billing spread across test results is only ~$185; LOS spread across medications is <0.1 day | No exploitable clinical-cost correlation exists in this dataset | Flag as a **data-quality issue** for audit, not a clinical finding |

**Bottom line:** *Fix long-stay capacity → Monitor financial risk → Validate the data.*

### Supporting metrics at a glance

| Metric | Value | YoY |
|---|---|---|
| Total Patients | 55K | ▲ 24.9% |
| Total Hospitals | 40K | ▲ 21.8% |
| Total Billing | $1bn+ | ▲ 25.0% |
| Avg. Billing per Patient | $26K | ▲ 0.1% |
| Avg. Length of Stay | 15.5 days | ▲ 0.1% |
| Long-Stay % (>21 days) | 29.8% | ▲ 0.2% |
| Abnormal Test Rate | 33.5% | ▲ 0.0% |
| Top Condition | Diabetes | — |
| Top Payer | Cigna (20.3% share) | — |

---

## 🏗️ Project Architecture

```
                ┌─────────────────────┐
                │  Raw CSV Extract     │
                │  healthcare_dataset  │
                └──────────┬──────────┘
                           │
                 (1) Data Cleaning
             _Healthcare_Data_Cleaning.ipynb
             • column standardization
             • dtype/date parsing
             • feature engineering (LOS, age_group)
                           │
                           ▼
                ┌─────────────────────┐
                │  Clean CSV Layer     │
                │  healthcare_clean    │
                └──────────┬──────────┘
                           │
              (2) Exploratory Analysis         (3) Database Load
                   02_EDA.ipynb          03_Healthcare_SQLAlchemy_Load.ipynb
              • distributions, trends         • SQLAlchemy + PyMySQL
              • correlation analysis           • loads into MySQL (health_data)
                           │                             │
                           ▼                             ▼
                  /image (chart exports)        MySQL: healthcare_db
                                                          │
                                              (4) SQL validation — 03_sql.ipynb
                                                          │
                           ┌──────────────────────────────┘
                           ▼
                ┌─────────────────────┐
                │   Power BI Model     │
                │  8-page dashboard    │
                │  (measures & DAX,    │
                │   cross-page filters)│
                └──────────┬──────────┘
                           │
                           ▼
              Executive Recommendations
           (signal → evidence → impact → action)
```

---

## 📂 Repository Structure

```
Healthcare Patient Intelligence & Hospital Performance Analytics/
│
├── Data/
│   ├── Raw/                  # Original, unprocessed dataset (55,500 rows)
│   │   └── healthcare_dataset.csv
│   └── Clean/                 # Cleaned & feature-engineered dataset
│       └── healthcare_clean.csv
│
├── NoteBook/
│   ├── _Healthcare_Data_Cleaning.ipynb        # Cleaning & feature engineering
│   ├── 02_EDA.ipynb                           # Exploratory data analysis
│   ├── 03_sql.ipynb                           # SQL exploration & validation
│   └── 03_Healthcare_SQLAlchemy_Load.ipynb    # ETL: clean CSV → MySQL
│
├── Dashboards/
│   └── Healthcare Patient & Hospital Performance dashboards.pbix
│
├── image/                     # EDA chart exports (age, gender, billing trend, correlation)
├── Screenshot/                 # Full-page dashboard screenshots
└── README.md
```

---

## 🧬 Dataset Schema

| Field | Type | Description |
|---|---|---|
| `name` | string | Patient name |
| `age` | int | Patient age |
| `gender` | category | Male / Female |
| `blood_type` | category | A+, A-, B+, B-, AB+, AB-, O+, O- |
| `medical_condition` | category | Cancer, Obesity, Diabetes, Arthritis, Asthma, Hypertension |
| `date_of_admission` / `discharge_date` | date | Admission and discharge dates |
| `doctor` | string | Attending physician |
| `hospital` | string | Facility name |
| `insurance_provider` | category | Aetna, Blue Cross, Cigna, Medicare, UnitedHealthcare |
| `billing_amount` | float | Total billed amount |
| `room_number` | int | Assigned room |
| `admission_type` | category | Emergency / Urgent / Elective |
| `medication` | category | Aspirin, Ibuprofen, Lipitor, Paracetamol, Penicillin |
| `test_results` | category | Normal / Abnormal / Inconclusive |
| `length_of_stay` *(engineered)* | int | `discharge_date − date_of_admission`, in days |
| `age_group` *(engineered)* | category | Binned age brackets (0-18, 19-30, 31-45, 46-60, 61-75, 76+) |

---

## 🔬 Methodology

1. **Cleaning** — standardized column names to `snake_case`, normalized inconsistent name casing, parsed date fields, and validated categorical domains.
2. **Feature engineering** — derived `length_of_stay` (discharge − admission) and `age_group` (binned brackets) to enable operational and demographic cuts not present in the raw extract.
3. **Exploratory analysis** — profiled distributions (age, gender, billing, admission type) and ran correlation analysis across numeric fields to check for exploitable relationships between clinical and financial variables.
4. **Persistence** — loaded the clean dataset into MySQL via SQLAlchemy, with connection parameters externalized as environment variables for reproducibility across environments.
5. **Modeling & visualization** — built a Power BI semantic model with DAX measures for YoY deltas, share-of-total calculations, and long-stay bed-day concentration, wired to synchronized slicers across all 8 pages.
6. **Synthesis** — translated the strongest signals into a four-priority executive recommendation framework (signal → evidence → impact → action), rather than presenting metrics without a call to action.

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10+
- MySQL Server 8.0+
- Power BI Desktop (Windows) to open the `.pbix` file

### 1. Clone & install dependencies
```bash
git clone <your-repo-url>
cd "Healthcare Patient Intelligence & Hospital Performance Analytics"
pip install pandas numpy matplotlib sqlalchemy pymysql jupyter
```

### 2. Reproduce the data pipeline
```bash
jupyter notebook NoteBook/_Healthcare_Data_Cleaning.ipynb   # raw → clean CSV
jupyter notebook NoteBook/02_EDA.ipynb                      # generates /image charts
```

### 3. Load into MySQL
```bash
mysql -u root -p -e "CREATE DATABASE healthcare_db;"

export MYSQL_USER=root
export MYSQL_PASSWORD=your_password       # never hardcode this — see Security below
export MYSQL_HOST=localhost
export MYSQL_PORT=3306
export MYSQL_DATABASE=healthcare_db

jupyter notebook NoteBook/03_Healthcare_SQLAlchemy_Load.ipynb
```

### 4. Explore the dashboard
Open `Dashboards/Healthcare Patient & Hospital Performance dashboards.pbix` in Power BI Desktop. Use the **Year / YearMonth / medical_condition / hospital / admission_type** slicers on every page for cross-filtered drill-down.

---

## ⚠️ Data Quality & Limitations

Self-disclosed directly on the relevant dashboard pages so readers don't misinterpret the numbers:

- **Entity uniqueness** — hospital and doctor names are near-unique per record; repeat encounters are negligible (0.04% of patients), so readmission-style analysis should be treated as a data-shape limitation rather than a clinical outcome.
- **Partial calendar years** — 2019 and 2024 are partial years in the dataset; exclude them or use like-for-like periods when computing YoY comparisons.
- **No clinical-cost correlation** — billing and length-of-stay show limited variation across medications and test results, which reads as a data-generation artifact (this is a synthetic dataset) rather than a clinical finding.
- **Synthetic data** — this dataset is demo-grade and used here to showcase the analytics pipeline and dashboard design, not for real clinical or financial decision-making.

---

## 🔐 Security

> The notebooks were originally authored with a hardcoded MySQL credential. **Before publishing this repo**, confirm that:
1. All credentials are read via `os.getenv(...)` (already partially implemented) rather than hardcoded strings.
2. No secrets remain in git history — if any were committed, rotate the credential and scrub history with `git filter-repo` or the BFG Repo-Cleaner.
3. A `.env.example` (not `.env`) is committed, and real `.env` files are gitignored.

---

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| Data wrangling & EDA | Python, Pandas, NumPy, Matplotlib |
| ETL / persistence | SQLAlchemy, PyMySQL |
| Database | MySQL |
| Visualization / BI | Power BI (DAX measures, cross-page slicers) |
| Environment | Jupyter Notebook |

---

## 🗺️ Roadmap

- [ ] Parameterize the LOS-reduction KPI as a live measure with alerting thresholds
- [ ] Add a data-quality validation notebook (schema checks, null/outlier audits) as a pre-load gate
- [ ] Migrate credential handling fully to `.env` / secrets manager
- [ ] Add automated tests for the cleaning pipeline (`pytest`)
- [ ] Publish the Power BI report to a Power BI Service workspace with scheduled refresh
- [ ] Add a CI workflow (GitHub Actions) to lint notebooks and validate the cleaning pipeline on push

---

## 🤝 Contributing

Contributions are welcome. Please:
1. Fork the repo and create a feature branch (`git checkout -b feature/your-feature`)
2. Keep notebook cells idempotent and avoid committing credentials or large binary outputs
3. Open a pull request describing the change and its impact on the pipeline or dashboard

---

## 📄 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

## 🙋 Author

**Your Name** — [LinkedIn](#) • [Portfolio](#) • [Email](#)

If you found this project useful, consider ⭐ starring the repo.
