# HealthConnect Clinic — Reducing Missed Appointments with Data & AI

**AnalystLab Africa | Experience Lab Internship Programme**
**Data Science Track | Week 5: Project Kickoff & Problem Understanding**

---

## 📌 Project Overview

HealthConnect Clinic experiences a high rate of missed appointments (no-shows), leading to high costs, wasted appointment slots, inefficient resource allocation, and reduced patient care capacity.

This repository documents the **Data Science track** contribution to the shared, multi-track HealthConnect Experience Lab project. Other tracks (Project Management, Data Analytics, Machine Learning Engineering, Generative AI) are contributing in parallel from their own professional perspectives toward the same overarching business problem.

**Central project question:** How can HealthConnect Clinic use data and AI to reduce missed appointments and improve the patient support experience?

**Data Science track objective:** Define the machine learning problem, assess whether the available data can support a no-show prediction solution, and establish a defensible foundation for modelling.

---
 
## 🗂️ Project Progression

| Stage | Focus | Status |
|---|---|---|
| Problem Understanding | Business scenario review, ML problem definition, initial data assessment | ✅ Week 4 — Complete |
| Analysis & Development | Data preparation, feature engineering, baseline model development | ✅ Week 5 — Complete |
| Integration & Validation | Error analysis, cross-track validation, model refinement | ✅ Week 6 — Complete |
| Testing & Refinement | Cross-validation, overfitting checks, threshold tuning, cross-track evidence validation | ✅ Week 7 — Complete |
| Final Integration & Presentation | End-to-end handoff, final documentation | ⏳ Upcoming |
| Final Presentation | Consolidated project handoff | ⏳ Upcoming |
---

## 📂 Repository Structure

```
healthconnect-clinic-project/
│
├── README.md
├── requirements.txt
│
├── data/
│   ├── raw/
│   │   ├── HealthConnect_Appointment_Data.csv
│   │   └── HealthConnect_Data_Dictionary.xlsx
│   └── processed/
│       └── (cleaned/derived datasets will be added from Week 5 onward)
│
├── notebooks/
│   ├── Week4_ML_Problem_Definition.ipynb
│   ├── Week5_Baseline_Modelling.ipynb
│   ├── Week6_Model_Improvement_Validation.ipynb
│   └── Week7_Model_Testing_Refinement.ipynb
│
├── reports/
│   ├── Week4_Project_Summary.docx
│   ├── Week5_Project_Summary.docx
│   ├── Week6_Project_Summary.docx
│   ├── Week7_Project_Summary.docx
│   ├── Week7_Testing_Validation_Evidence.docx
│   └── Week7_HCPOD_Cross_Track_Evidence.docx
│
└── resources/
    └── HealthConnect_Clinic_Knowledge_Base.docx   # reference only — Generative AI track resource
```
---

## 🧬 Dataset Description

**File:** `HealthConnect_Appointment_Data.csv`
**Size:** 5,000 appointment records × 18 fields
**Verified (Week 4):** 0 duplicate records, 0 duplicate appointment IDs; missing values present in 3 fields only

**Covers:** patient demographics, appointment and booking details, prior appointment/no-show history, reminder information, distance to clinic, waiting time, and appointment outcome.

Full field-level definitions are provided in `HealthConnect_Data_Dictionary.xlsx`.

---

## 🎯 Week 4 — Machine Learning Problem Definition

### Problem Framing
Framed as a **binary classification problem**: predicting, at or shortly after booking time, whether a patient will attend (`Attended`) or miss (`No-Show`) their scheduled appointment.

### Key Findings from Initial Data Assessment

| Area | Finding |
|---|---|
| Target variable | `appointment_outcome` contains **3 classes** — No-Show (48.5%), Attended (46.3%), Cancelled (5.3%) — not a simple binary field as might be assumed |
| Missing data | 3 fields affected: `reminder_channel` (1,366 — structurally tied to `reminder_sent = No`), `distance_to_clinic_km` (90), `waiting_time_minutes` (60) |
| Data leakage risk | `waiting_time_minutes` is populated even for No-Show records with near-identical means across all outcomes (~24 min) — flagged as a data realism limitation and excluded from the candidate feature set |
| Strongest early signal | `previous_no_shows` shows the clearest behavioral difference between outcome groups (0.64 vs. 0.46 mean) |
| Class balance | After excluding Cancelled appointments, the binary target is well balanced (51.2% No-Show / 48.8% Attended) — resampling is not expected to be necessary |

### Proposed Target Variable
`appointment_outcome`, binarized as **Attended vs. No-Show**, with Cancelled appointments (5.3%) excluded from the primary model as a distinct, planned patient action outside the scope of unplanned no-show prediction.

### Candidate Features
`age`, `gender`, `appointment_type`, `booking_lead_days`, `previous_appointments`, `previous_no_shows`, `reminder_sent`, `reminder_channel`, `distance_to_clinic_km`, `appointment_day`, `appointment_time`

**Excluded:** `appointment_id`, `patient_id` (identifiers); `waiting_time_minutes` (leakage/realism concern); `age_group` (redundant with `age`); raw date fields (represented via derived fields already in the candidate list).

### Proposed Initial Modelling Approach
- Baseline: Logistic Regression (interpretable)
- Comparison model: a tree-based approach (Random Forest or Gradient Boosting) to capture non-linear interactions
- Evaluation: accuracy alongside precision/recall/F1, given the asymmetric operational cost of false negatives (missing a true no-show prediction has a different cost than a false alarm)

Full methodology, code, and real output are documented in [`notebooks/Week4_ML_Problem_Definition.ipynb`](notebooks/Week4_ML_Problem_Definition.ipynb).

---

## ⚠️ Key Considerations, Assumptions & Limitations

- **Synthetic data limitation:** the `waiting_time_minutes` anomaly (populated for no-show patients, which is not logically possible in a real clinic) confirms this dataset does not perfectly mirror real-world clinical behavior — findings should be validated cautiously before assuming real-world generalization
- **Possible circularity risk:** reminder-related fields may reflect existing staff judgement about patient risk rather than a purely independent effect — worth revisiting during feature importance analysis in later weeks
- **Sample size:** 5,000 records may limit reliable subgroup analysis (e.g., by `appointment_type`) in later modelling stages
- **Cross-track dependency:** feature and deployment decisions may need to align with parallel work from the Machine Learning Engineering track once that track's system design is finalized 

---

## 🛠️ Tools & Libraries

- **Language:** Python 3
- **Environment:** Jupyter Notebook
- **Core libraries:** `pandas`, `numpy`

Additional libraries (`scipy`, `scikit-learn`, `matplotlib`, `seaborn`) will be introduced from Week 5 onward as cleaning, EDA, and modelling work begins. 

---

## 🚀 How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/healthconnect-clinic-project.git
   cd healthconnect-clinic-project
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Launch the Week 4 notebook:
   ```bash
   jupyter notebook notebooks/Week4_ML_Problem_Definition.ipynb
   ```

4. Run all cells sequentially to reproduce the data assessment and problem definition findings.

---

## 📈 Proposed Focus for Week 5

Move into the Analysis & Solution Design stage: formal data cleaning (structured handling of the three missing-value fields), advanced EDA, statistical validation of the signals identified in Week 4 (e.g., testing whether `booking_lead_days` and `previous_no_shows` differences are statistically significant), and a formal feature preprocessing plan ahead of baseline model development.

---

## 🤖 Week 5 — Data Preparation, Feature Engineering & Baseline Model Development

**Objective:** Move from Week 4's problem definition into practical baseline model development.

### Data Preparation
- `reminder_channel` missingness (structural, tied to `reminder_sent = No`) filled with an explicit `'Not Sent'` category
- `distance_to_clinic_km` missingness (90 rows, spread at random) filled with median
- Date fields corrected to proper `datetime` type
- Identifiers, `age_group` (redundant with `age`), and `waiting_time_minutes` (confirmed Week 4 leakage/realism risk) removed

### Feature Engineering
| Feature | Description |
|---|---|
| `is_new_patient` | Binary flag for the 4.8% of patients with zero prior appointments |
| `prior_no_show_rate` | Previous no-shows ÷ previous appointments; new patients assigned the dataset-wide average rather than 0 |
| `lead_time_bucket` | `booking_lead_days` binned into Short/Medium/Long using real data quartiles (15/30/45 days) |

### Train/Test Strategy
Stratified random 80/20 split — chosen after confirming no-show rates showed no meaningful drift across the dataset's 18-month date range, meaning a time-based split wasn't necessary at this baseline stage (noted as a Week 6 consideration).

### Baseline Model Results (real, executed output)

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Logistic Regression | 0.616 | 0.614 | 0.672 | 0.642 | **0.669** |
| Random Forest | 0.600 | 0.604 | 0.633 | 0.618 | 0.635 |

**Key finding:** Logistic Regression outperformed Random Forest on every metric, suggesting the relationship between features and no-show risk is largely linear/additive rather than driven by complex interactions — supporting Logistic Regression as the lead model going into Week 6. Top predictors: `booking_lead_days` and `previous_no_shows` (both positive), consistent with Week 4's informal signal checks.

### Decision Validation: Cancelled Appointment Handling
Proposed excluding Cancelled appointments (5.3%) from the model rather than merging them into No-Show. Week 5 tested this decision empirically rather than assuming it: a merged-target model showed slightly weaker performance (ROC-AUC 0.657 vs. 0.669), and behavioral data confirmed Cancelled patients have a *lower* average prior no-show rate (0.418) than even Attended patients (0.457) — meaningfully different from true No-Show patients (0.641). **Decision confirmed, not changed**, with new evidence now backing the original Week 4 proposal.

### Cross-Track Collaboration
Coordinated with the Data Analytics track: their focus on no-show patterns by reminder status and distance to clinic informed the decision to retain `reminder_sent`, `reminder_channel`, and `distance_to_clinic_km` as candidate features despite modest individual effect sizes.

---

## 📈 Proposed Focus for Week 6

Refine the Logistic Regression baseline through formal feature importance analysis and hyperparameter tuning, test a time-based train/test split for production-realistic evaluation, and coordinate with the Machine Learning Engineering track on expected model output format ahead of integration work.

---

## 🔗 Week 6 — Model Improvement, Error Analysis & Validation

**Objective:** Improve and validate the Week 5 baseline through error analysis and a genuine cross-track integration.

### Real Cross-Track Integration
A Data Analytics track intern (Audrey Mphisa) shared five independent descriptive findings and six direct questions about the Week 5 feature set. Her data was verified against my findings first, then an ablation study answered each question with evidence:

| Question | Finding |
|---|---|
| Does `lead_time_bucket` add value beyond `booking_lead_days`? | No — redundant (removed) |
| Does `prior_no_show_rate` add value beyond `previous_no_shows`? | Modest yes — both retained |
| Does `is_new_patient` add value beyond `previous_appointments`? | No — redundant (removed) |
| Do `reminder_sent`/`reminder_channel` materially improve prediction? | No, and they're **perfectly collinear** — `reminder_sent` removed |
| Does distance remain important after other variables are considered? | Statistically significant individually, but negligible contribution to overall discrimination |
| Which features are most important? | `booking_lead_days`, `previous_no_shows`, `distance_to_clinic_km`, `prior_no_show_rate`, `previous_appointments` (only these reach p<0.05) |

### Error Analysis
False negatives (missed no-shows) are concentrated among patients with lower `prior_no_show_rate` and `previous_no_shows` than those with correctly identified no-shows. This indicates that the model is weakest for patients without an obvious risk history, reflecting a structural limitation of a history-driven feature set.

### Refined Model Results (real, executed output)

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Week 5 Baseline (Logistic Regression) | 0.616 | 0.614 | 0.672 | 0.642 | 0.669 |
| Week 6 Refined Logistic Regression | 0.615 | 0.613 | **0.672** | 0.641 | 0.668 |
| Week 6 Refined Gradient Boosting | 0.617 | 0.617 | 0.662 | 0.639 | 0.661 |

**Candidate model recommendation:** Refined Logistic Regression — matches the baseline's recall exactly (the operationally critical metric, since a missed no-show costs more than a false alarm) while using a leaner, non-collinear, statistically justified feature set (11 fields vs. 14).

**Business relevance:** At ROC-AUC ~0.67, the model is suitable as a risk-flagging aid for staff attention — not yet suitable as a fully autonomous intervention system.

---

## 📈 Proposed Focus for Week 7

Run k-fold cross-validation on the refined Logistic Regression model, conduct a full VIF audit across the refined feature set, test classification threshold adjustments given the asymmetric cost of false negatives, and confirm the refined feature list with the Data Analytics track intern.

## 🧪 Week 7 — Model Testing, Error Analysis & Refinement

**Objective:** Systematically test whether the Week 6 candidate model's performance was genuine or a single-split artifact, check for overfitting, validate segment consistency, and refine based on evidence.

### 5-Fold Cross-Validation (real, executed output)

| Feature Set | Mean Test AUC | Std |
|---|---|---|
| Week 5 Baseline (14 features) | 0.6793 | 0.0120 |
| Week 6 Refined (11 features) | 0.6793 | 0.0108 |

Paired t-test across folds: **p = 0.9544** — confirms the Week 6 feature refinement is genuinely equivalent, not a single-split coincidence.

### Overfitting Check

| Model | Train AUC | Test AUC | Gap |
|---|---|---|---|
| Logistic Regression | 0.6934 | 0.6678 | 0.0256 (healthy) |
| Gradient Boosting | 0.7574 | 0.6613 | **0.0961 (overfitting)** |

**Gradient Boosting is now definitively ruled out** as a candidate model.

### Segment Analysis (new input)
- Patient history and gender segments: broadly consistent (with a small-sample caveat on "Prefer not to say," n=16)
- **Real inconsistency found:** Specialist Consultation appointments (AUC 0.588) underperform relative to General Consultation (0.696) — flagged for Week 8, not resolved this week

### Threshold Tuning — the clearest improvement found in this project so far

| Threshold | Recall | Precision | F1 |
|---|---|---|---|
| Default (0.5) | 0.672 | 0.613 | 0.641 |
| Tuned (0.478, Youden's J) | **0.730** | 0.612 | 0.666 |

False negatives (missed no-shows) dropped from 159 to 131 at negligible precision cost.

### HC-POD Cross-Track Testing 
1. **Retested** the Week 6 Data Analytics-informed feature decisions under 5-fold CV — confirmed robust
2. **Validated a real metric discrepancy** raised by a Data Analytics intern (64.1% vs. 70.5% for a shared Lead Time × Previous No-Show segment): confirmed the 70.5% figure's exact denominator, re-ran with Cancelled included (67.88%), and honestly reported a **partial, not full, reconciliation** — a ~3.8 point gap remains unexplained pending the analyst's exact segment definition

**Candidate model recommendation:** Logistic Regression, 11-feature refined set, **tuned threshold ~0.478**.

---

## 📈 What Must Be Completed Before Week 8

Obtain the Data Analytics intern's exact segment filter logic to fully reconcile the remaining discrepancy; investigate the Specialist Consultation segment weakness; finalize and hand off the candidate model (with its tuned threshold, not just the model object) to ML Engineering.


## 🙋 Author

**Donald Nwachukwu**
Data Science Track, AnalystLab Africa Experience Lab

---

#AnalystLabAfrica
