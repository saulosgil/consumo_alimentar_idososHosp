# 🍽️🧓 consumo_alimentar_idososHosp

Repository with notebooks and supporting materials for the study investigating **food intake of hospitalized older adults** and its influence on **muscle** and **clinical outcomes**.

## 📖 About the project

This repository organizes the analytical workflow of a **prospective observational cohort study** conducted at the Geriatrics ward of the *Hospital das Clínicas da Faculdade de Medicina da Universidade de São Paulo* (HCFMUSP), Brazil.

Older adults frequently fail to reach the energy and protein targets recommended during hospitalization. Combined with anabolic resistance, physical inactivity, and systemic inflammation, low intake may accelerate muscle loss and worsen clinical prognosis. This study tests whether patients whose intake is closer to the prescribed nutritional target show better muscle and clinical outcomes at hospital discharge.

- **Registration**: ClinicalTrials.gov — [NCT06940284](https://clinicaltrials.gov/study/NCT06940284)
- **Ethics approval**: HCFMUSP Research Ethics Committee (CAAE: 66834923300000068; approval nº 5.900.093)

## 🎯 Study objective

1. To characterize the **energy and protein intake** of older adults during hospitalization;
2. To examine the influence of energy-protein intake, at hospital discharge, on:
   - cross-sectional area of the **rectus femoris** and **vastus lateralis** muscles;
   - **handgrip strength**;
   - **physical function** (SPPB);
   - **level of independence** (Barthel Index);
   - **length of hospital stay** and **in-hospital mortality**;
3. To verify whether energy-protein intake during hospitalization is a significant **predictor** of the loss of muscle mass, strength, physical function, and independence at discharge.

## 🧪 Study design

**Prospective observational cohort.** Assessments were performed at **admission** (< 12 h after ward admission, ADM) and at **medical discharge** (ALTA).

### Eligibility

- **Inclusion**: men and women aged ≥ 65 years; admitted to the ward within the last 12 hours; able to feed orally.
- **Exclusion**: history of cancer in the last five years; musculoskeletal or neurological disorders preventing assessments; enteral or parenteral feeding.

### Main measures assessed

- **Habitual diet**: food frequency questionnaire (FFQ), following Fisberg et al. (2008);
- **In-hospital food intake**: **weighed food method** (reference method) for breakfast, lunch, and dinner every day of hospitalization. Each food item was weighed before and after consumption (PRIX® 9094 Plus scale), and energy and macronutrients were computed in **Nutritionist Pro®** (v7.3, USDA database), complemented by Brazilian food composition references;
- **Nutritional targets** (ESPEN guideline on hospital nutrition, 2021): **30 kcal/kg/day** and **1.2–2.0 g protein/kg/day**. For patients with obesity (BMI ≥ 30 kg/m²), an adjusted reference weight was used: *ideal weight (BMI 25) + 0.25 × (current weight − ideal weight)*;
- **Muscle mass**: B-mode ultrasound (SonoAce R3, 7.5 MHz) with panoramic reconstruction of rectus femoris and vastus lateralis cross-sectional area, analyzed in ImageJ by a single blinded assessor;
- **Physical function**: Short Physical Performance Battery (**SPPB**; balance, 3-m gait speed, and 5× chair stand; 0–12 points);
- **Muscle strength**: **handgrip strength** (Takei TKK 5101 dynamometer; best of 3 attempts);
- **Independence**: **Barthel Index** (0–100);
- **Biochemical parameters** (from medical records): CRP, lipid profile, liver enzymes, glucose, insulin, HbA1c, total protein and fractions, and 25(OH)D;
- **Clinical outcomes**: length of hospital stay and in-hospital mortality.

## 🥗 Intake classification

Patients are classified according to the **percentage of the daily target** (energy or protein) reached during hospitalization. Only days with recorded intake (numeric values in the `cons_*_dayX` columns) count as hospitalization days.

| Variable | Group | Criterion |
| --- | --- | --- |
| `TresGruposKcal` | **ProxMeta** | ≥ 70% of the target on at least 50% of the days |
|  | **MetMeta** | not ProxMeta, but ≥ 50% of the target on at least 50% of the days |
|  | **AbaixoMeta** | < 50% of the target on more than 50% of the days |
| `DoisGruposKcal` | **ProxMeta** | ≥ 50% of the target on at least 50% of the days |
|  | **MetMeta** | remaining patients |

Groups are assigned hierarchically, so each patient falls into a single category. Patients with no recorded intake are left unclassified (`NaN`). The same rules are applied to energy (`meta_kcal`) and protein (`meta_ptn`).

## 📊 Statistical analysis

- **Descriptive statistics** (mean, median, SD, minimum, and maximum) of daily and per-meal (breakfast, lunch, dinner) energy and protein intake;
- Screening for outliers and testing of sphericity, homoscedasticity, and normality;
- **Mixed models for repeated measures** with group (intake tertiles: 1st, 2nd, 3rd) and time (ADM and ALTA) as fixed factors and subjects as random factors, followed by **Tukey** post-hoc tests;
- **Regression models** (crude and adjusted for age, sex, BMI, diabetes, hypertension, congestive heart failure, chronic kidney disease, COPD, and chronic neurological disease) to test intake relative to target as a predictor of muscle and clinical outcomes, reported as **β** with 95% CI and P value;
- Significance level: **P ≤ 0.05**.

## 🤖 Technologies used

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)

## 📁 Repository structure

| File / folder | Description |
| --- | --- |
| `data/` | Study datasets (not versioned — see note below). |
| `classificar_consumo_kcal.ipynb` | Classifies patients by the percentage of the **energy target** (`meta_kcal`) reached during hospitalization and saves `data/data_classificado.xlsx`. |
| `classificar_consumo_ptn.ipynb` | Classifies patients by the percentage of the **protein target** (`meta_ptn`) reached during hospitalization and saves `data/data_ptn_classificado.xlsx`. |

> 🔒 **Data availability**: the `data/` folder is listed in `.gitignore` because it contains participant information. Data may be made available upon reasonable request to the authors.

### Expected input format

| Column | Description |
| --- | --- |
| `meta_kcal` / `meta_ptn` | Daily energy (kcal) or protein (g) target of each patient. |
| `cons_kcal_dayX` / `cons_ptn_dayX` | Intake on hospitalization day *X* (e.g., `cons_kcal_day1`, `cons_kcal_day2`, …). Empty cells mean no record for that day. |

## 🔁 Reproducibility

The analyses were performed in **Python** using Jupyter notebooks. With the datasets placed in `data/`, run:

1. `classificar_consumo_kcal.ipynb`
2. `classificar_consumo_ptn.ipynb`

Besides the group columns, each notebook stores auxiliary variables for checking (`dias_internados`, `prop_dias_70_ou_mais`, `prop_dias_50_a_69`, `prop_dias_abaixo_50`).

## 🗂 Purpose of this repository

This repository was created to:

- organize the analysis notebooks;
- document the study's analytical workflow;
- facilitate reproducibility of the analyses;
- centralize materials linked to the thesis and manuscript.

## 📑 Reference

📝 REIS, P. V. L. *Consumo alimentar de idosos hospitalizados e sua influência nos desfechos musculares e clínicos: um estudo de coorte prospectivo.* Faculdade de Medicina, Universidade de São Paulo, 2025.

Manuscript in preparation. The reference will be updated after publication.

---

## 👨‍💻 Author

**Saulo Gil**
GitHub: [@saulosgil](https://github.com/saulosgil)
