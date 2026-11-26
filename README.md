# DAV 6150: Data Science — Course Dataset Repository

[![Course](https://img.shields.io/badge/Course-DAV%206150%20Data%20Science-1F4E79)](https://www.yu.edu/)
[![Format](https://img.shields.io/badge/Data-CSV-217346?logo=microsoftexcel)](#2-dataset-directory)
[![Analysis](https://img.shields.io/badge/Analysis-Python%20%2F%20pandas-3776AB?logo=python)](https://pandas.pydata.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

A central data repository for the **DAV 6150 Data Science** course in my graduate studies at **Yeshiva University**. It hosts the module, project and final-project datasets as raw CSV files so that notebooks can load them directly over HTTPS from GitHub, with no local downloads. The datasets span **regression, count modelling, binary classification and model-evaluation** problems across wine sales, online news popularity, diabetes screening, customer loyalty, e-commerce sessions, school graduation outcomes and census labour data.

---

## 1. How the Repository Is Used

### Data Access Flow
$$\text{CSV in this repo} \xrightarrow{\text{raw.githubusercontent.com}} \text{pd.read\_csv()} \xrightarrow{} \text{Jupyter / Colab notebook}$$

Each analysis notebook reads its dataset straight from the raw GitHub URL, which keeps notebooks reproducible on any machine:

```python
import pandas as pd

BASE = "https://raw.githubusercontent.com/alwyn-munatsi/DAV-6150/main"
df = pd.read_csv(f"{BASE}/FinalProject_Data.csv")
```

For example, the final project — [machine-learning-predict-hours-per-week](https://github.com/alwyn-munatsi/machine-learning-predict-hours-per-week) — loads `FinalProject_Data.csv` this way.

---

## 2. Dataset Directory

| File | Rows | Columns | Subject | Target / Label | Problem Type |
| :--- | ---: | ---: | :--- | :--- | :--- |
| **`Module3/M3_Data.csv`** | 12,795 | 16 | Wine chemistry and marketing | `TARGET` (cases sold, 0–8) | Count regression |
| **`Module4/M4_Data.csv`** | 39,644 | 61 | Mashable online news articles | `shares` | Regression (popularity) |
| **`Module5/M5_Data.csv`** | 181 | 11 | Diabetes screening (Pima-style) | `class` with `scored.class`, `scored.probability` | Classification metrics |
| **`Module7/M7_Data.csv`** | 14,016 | 15 | Customer loyalty / product holdings | `TARGET` (Y / N) | Binary classification |
| **`M11_Data.csv`** | 73,152 | 19 | New York school graduation outcomes, 2018–19 | `grad_pct`, `reg_pct`, `dropout_pct` | Regression / classification |
| **`Project1/Project1_Data.csv`** | 0 | — | Placeholder (currently empty) | — | — |
| **`Project2/Project2_Data.csv`** | 12,330 | 17 | Online shopper sessions | — (labels in separate file) | Binary classification |
| **`Project2/Project2_Data_Labels.csv`** | 12,330 | 1 | Purchase labels for Project 2 | `Revenue` (TRUE / FALSE) | — |
| **`FinalProject_Data.csv`** | 32,561 | 15 | UCI Adult census (1994) | `hours_per_week` (also `income`) | Regression |

---

## 3. Dataset Details

### Module 3 — Wine Sales (`M3_Data.csv`)
Chemical properties (`FixedAcidity`, `VolatileAcidity`, `CitricAcid`, `ResidualSugar`, `Chlorides`, sulfur dioxide, `Density`, `pH`, `Sulphates`, `Alcohol`), marketing features (`LabelAppeal`, `AcidIndex`, `STARS`) and `TARGET`, the number of cases purchased. Contains missing values (e.g. `FreeSulfurDioxide`) and negative chemistry values, making it a good cleaning and imputation exercise. Most rows fall between 2 and 5 cases, with a large spike at 0.

### Module 4 — Online News Popularity (`M4_Data.csv`)
Article URL, `timedelta` and 58 content features: token counts, link/image/video counts, keyword statistics, data channel flags, weekday flags, LDA topic weights and sentiment/subjectivity scores. The target is `shares`. Note that the header names contain a leading space (e.g. `" shares"`), so strip column names after loading.

### Module 5 — Classification Output (`M5_Data.csv`)
Clinical measures (`pregnant`, `glucose`, `diastolic`, `skinfold`, `insulin`, `bmi`, `pedigree`, `age`), the actual `class`, and a model's `scored.class` and `scored.probability`. Used for computing a confusion matrix, accuracy, precision, recall, F1, ROC and AUC by hand.

### Module 7 — Customer Loyalty (`M7_Data.csv`)
`loyalty`, `age`, `city`, length of relationship (`LOR`), product holdings (`prod_A`, `prod_B`, `type_A`, `type_B`), turnover (`turnover_A`, `turnover_B`), `contract`, plus `TARGET`. Classes: 8,000 `N` vs 6,016 `Y`.

### Module 11 — NY School Graduation Outcomes (`M11_Data.csv`)
2018–19 cohort outcomes by district/school (`aggregation_type`, `aggregation_name`), needs category (`nrc_desc`), county, NYC flag, cohort description and student subgroup, with enrolment, graduation, Regents diploma and dropout counts and percentages. Percentages are stored as strings like `71%`; convert to numbers before modelling.

### Project 2 — Online Shoppers (`Project2_Data.csv` + `Project2_Data_Labels.csv`)
Page-type counts and durations (`Administrative`, `Informational`, `ProductRelated`), `BounceRates`, `ExitRates`, `PageValues`, `SpecialDay`, `Month`, browser/OS/region/traffic type, `VisitorType` and `Weekend`. The `Revenue` labels are in a separate file aligned row-by-row.

### Final Project — UCI Adult (`FinalProject_Data.csv`)
`age`, `workclass`, `fnlwgt`, `education`, `education_num`, `marital_status`, `occupation`, `relationship`, `race`, `sex`, `capital_gain`, `capital_loss`, `hours_per_week`, `native_country`, `income`. Categorical values have a leading space (e.g. `" Private"`) and missing values are coded as `" ?"`.

---

## 4. Data Quality Notes

| Dataset | Issue | Suggested Handling |
| :--- | :--- | :--- |
| `M3_Data.csv` | Missing chemistry values; file starts with a UTF-8 BOM | Impute; read with `encoding="utf-8-sig"` |
| `M4_Data.csv` | Column names have leading spaces | `df.columns = df.columns.str.strip()` |
| `M11_Data.csv` | Percentages stored as text | `df[col].str.rstrip('%').astype(float)` |
| `Project2` | Features and labels in separate files | `pd.concat([X, y], axis=1)` |
| `FinalProject_Data.csv` | Leading spaces in categories; `" ?"` for missing | `str.strip()`, then replace `?` |
| `Project1_Data.csv` | Empty file | Not usable yet |

---

## 5. Repository Structure

```text
DAV-6150/
├── README.md
├── LICENSE                         <-- MIT
├── FinalProject_Data.csv           <-- UCI Adult census (final project)
├── M11_Data.csv                    <-- NY graduation outcomes
├── Module3/M3_Data.csv             <-- Wine sales
├── Module4/M4_Data.csv             <-- Online news popularity
├── Module5/M5_Data.csv             <-- Scored classification output
├── Module7/M7_Data.csv             <-- Customer loyalty
├── Project1/Project1_Data.csv      <-- Empty placeholder
└── Project2/
    ├── Project2_Data.csv           <-- Online shopper sessions
    └── Project2_Data_Labels.csv    <-- Revenue labels
```

---

## 6. Step-by-Step Usage Guide

### Option A: Load Directly from GitHub
```python
import pandas as pd
BASE = "https://raw.githubusercontent.com/alwyn-munatsi/DAV-6150/main"

wine     = pd.read_csv(f"{BASE}/Module3/M3_Data.csv", encoding="utf-8-sig")
news     = pd.read_csv(f"{BASE}/Module4/M4_Data.csv"); news.columns = news.columns.str.strip()
shoppers = pd.concat([pd.read_csv(f"{BASE}/Project2/Project2_Data.csv"),
                      pd.read_csv(f"{BASE}/Project2/Project2_Data_Labels.csv")], axis=1)
```

### Option B: Clone Locally
```bash
git clone https://github.com/alwyn-munatsi/DAV-6150.git
cd DAV-6150
```
Then point `pd.read_csv()` at the local paths shown in Section 5.

---

## License
MIT — see [LICENSE](LICENSE). The datasets come from public sources and course materials (for example, the UCI Machine Learning Repository) and remain subject to their original terms.
