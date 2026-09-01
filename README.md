# HealthConnect Clinic — Reducing Missed Appointments with Data & AI

**AnalystLab Africa | Experience Lab Internship Programme**
**Data Science Track | Week 4: Project Kickoff & Problem Understanding**

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
| Analysis & Solution Design | Formal cleaning, advanced EDA, statistical validation, preprocessing plan | ⏳ Upcoming |
| Development | Model building and training | ⏳ Upcoming |
| Testing & Refinement | Model evaluation and iteration | ⏳ Upcoming |
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
│   └── Week4_ML_Problem_Definition.ipynb
│
├── reports/
│   └── Week4_Project_Summary.docx
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

## ⚠️ Key Modelling Considerations, Assumptions, Limitations & Risks
**Assumptions:**
Cancelled appointments are treated as a distinct patient action, separate from unplanned no-shows, for the purposes of this initial model
Missing values in distance_to_clinic_km and waiting_time_minutes is assumed to be at random, based on their proportional spread across outcome categories

**Limitations:**
This is a fictional, synthetic dataset — the waiting_time_minutes anomaly (populated for no-show patients) confirms the data does not perfectly mirror real-world clinic behavior, and findings should not be assumed to generalize to a live clinical setting without validation
5,000 records is a moderate sample size for a clinic-wide model; subgroup analysis (e.g., by appointment_type) may be limited by smaller subgroup sizes

**Risks:**
waiting_time_minutes was identified as a leakage risk and excluded — but similar risks should be re-checked for any newly engineered features in later phases
Reminder-related fields (reminder_sent, reminder_channel) may reflect clinic response to perceived risk (e.g., staff already flagging high-risk patients for reminders) rather than a purely independent cause — this potential circularity should be considered when interpreting feature importance later

**Dependencies:**
Final modelling work in later weeks depends on the Data Analytics track's parallel exploration of the same dataset, and any Machine Learning Engineering track decisions about deployment format, which may influence which features are practical to use in production  

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

## 🙋 Author

**Donald Nwachukwu**
Data Science Track, AnalystLab Africa Experience Lab

---

#AnalystLabAfrica
