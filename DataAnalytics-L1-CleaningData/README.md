# Task 3 — Cleaning Data (Student Records Dataset)

**Name:** Faith Aipoh
**Track:** Data Analytics
**Level:** 1
**Task:** 3

## 📌 Objective
Demonstrate professional-level data cleaning skills by taking a deliberately messy student records dataset and systematically transforming it into a clean, analysis-ready dataset. Every cleaning decision is documented and justified inline in the notebook.

## 🛠️ Tech Stack
- Python
- pandas
- numpy
- Jupyter Notebook

## 📂 Dataset
Student records dataset containing 1,540 rows with the following columns:
`Student_ID`, `Student_Name`, `Age`, `Gender`, `Department`, `Phone`, `Email`, `Enrollment_Date`, `GPA`, `Attendance_%`, `Tuition_Fees`, `Status`

The raw dataset contained a mix of common real-world data quality issues: missing values, inconsistent formatting, out-of-range values, mixed date formats, and duplicate/conflicting identifiers.

## 🧹 Cleaning Process

| Column | Issues Found | How It Was Handled |
|---|---|---|
| **Student_Name** | 16 missing values | Filled with `'Unknown'` placeholder — name is an identifier, not suitable for statistical imputation |
| **Age** | Non-numeric placeholders (`'unknown'`, `'?'`), a `99` sentinel value | Coerced to numeric, `99` treated as invalid, missing values filled with **median** |
| **Gender** | Inconsistent casing/typos (`'M'`, `'mle'`, `'FEMLE'`, etc.) | Lowercased, stripped, and mapped to standardized `Male`/`Female` |
| **Department** | Inconsistent casing, typos, missing/blank/placeholder entries | Standardized to 5 canonical categories; missing values set to `'UNKNOWN'` |
| **Phone** | Mixed formats (local, country code, parentheses), missing values, wrong-length entries | Standardized to digits-only format; missing → `'Not Provided'`; invalid-length numbers → `'Invalid'` (kept distinct from missing) |
| **Email** | Whitespace, malformed domains (`'@example'` missing `.com`), missing values | Stripped/lowercased, malformed addresses corrected where pattern was unambiguous, missing → `'Not Provided'` |
| **Enrollment_Date** | Four different date formats mixed together, plus missing values | Custom parser tried each known format in sequence; unparseable/missing values kept as `NaT` to preserve datetime dtype |
| **GPA** | Non-numeric placeholders, out-of-range values (negative, >5.0 on a 0–5 scale) | Coerced to numeric, out-of-range values treated as missing, imputed with **median** |
| **Attendance_%** | Non-numeric placeholders, values >100% | Coerced to numeric, impossible values (>100%) treated as missing, imputed with **median** |
| **Tuition_Fees** | Stored as text, negative values, outliers | Coerced to numeric; negative values treated as missing; imputed with **department-level median** to reflect fee variation across programs |
| **Status** | Inconsistent casing and a spelling typo (`'Actve'`) | Standardized to 4 clean categories: `Graduated`, `Active`, `Suspended`, `On Leave` |

### Duplicates
- Checked for full-row duplicates and dropped exact matches.
- Identified `Student_ID` collisions (different students sharing the same ID) — these were **not** treated as duplicates, since dropping either row would mean losing real student data. Instead, new unique IDs were generated for the colliding records to restore `Student_ID` as a proper unique identifier.

### Data Type Correction
All columns were verified to have the correct dtype post-cleaning (numeric columns as `float64`/`int64`, `Enrollment_Date` as `datetime64`, categorical/text columns as `object`).

## 📊 Before vs. After Summary
A summary table comparing null counts, duplicate counts, row counts, and dtype accuracy before and after cleaning is included at the end of the notebook.

## 📁 Files
- `Data Cleaning.ipynb` — full cleaning notebook with markdown justification cells for every decision
- `data/` — contains both the raw messy dataset and the final cleaned output
  - `data/messy_student_records.csv` — original uncleaned dataset
  - `data/cleaned_student_records.csv` — final cleaned dataset

## 🎥 Demo
[Link to video walkthrough]

## 🔑 Key Takeaways
- Prioritized **transparency over automation** — every imputation/correction decision is justified in a markdown cell rather than applied silently.
- Distinguished between **missing** and **invalid** data throughout (e.g., phone numbers, GPA, attendance) rather than treating all anomalies the same way.
- Avoided fabricating values for identifier-type fields (name, email, phone, enrollment date) where imputation would misrepresent real data.
