# Predicting Lung Cancer Risk: A Machine Learning Approach Using Lifestyle and Health Indicators

Code artefact for an MSc Artificial Intelligence dissertation submitted to Manchester
Metropolitan University, September 2026.

|||
|-|-|
|**Author**|Avinash Pilania|
|**University ID**|25929861|
|**Programme**|MSc Artificial Intelligence|
|**Unit**|6G7V0007 MSc Project (60 credits)|
|**Supervisor**|Dr Nishanthi Abeynayake|
|**Theme**|Data Science Systems|
|**Ethics approval**|EthOS reference 92147|

\---

## Overview

Machine learning studies on a widely used 309-record lung cancer benchmark routinely report
near-perfect accuracy. This project investigates how much of that performance is genuine and
how much is an artefact of **symptom-based target leakage**, where symptom variables act as
proxies for disease that has already developed and could not be available to a genuine
pre-symptomatic screening instrument.

The central finding is that once leakage is measured and removed through disciplined feature
governance, a model built only on pre-symptomatic risk factors achieves statistically
indistinguishable discrimination from the full-feature model.

### Method in brief

Features were partitioned *a priori* into 8 pre-symptomatic risk factors and 7 symptom
indicators. A four-step investigation then followed:

1. **Diagnosis** — compare AUC across full, risk-only and symptom-only feature sets
2. **Significance** — paired bootstrap confidence interval on the observed gap
3. **Recovery** — retune all five classifiers on risk factors alone
4. **Confirmation** — test the residual gap with two independent procedures
(like-for-like bootstrap and nested cross-validation)

The analysis also covers probability calibration, subgroup fairness, cross-benchmark external
validation, SHAP and permutation explainability, and a measured carbon footprint.

\---

## Repository contents

```
├── lung\_cancer\_dissertation\_final.ipynb   Complete executable analysis pipeline
├── requirements.txt                       Dependency specification
├── README.md                              This file
└── data/
    ├── survey lung cancer (1).csv         Primary dataset (309 records)
    └── cancer patient data sets.csv       External validation dataset (1,000 records)
```

The notebook contains 99 cells (57 code, 42 markdown) with all outputs retained. Execution
counts run strictly in sequence from 1 to 57, and the stored run completed without errors.

\---

## Data

Both datasets are publicly available, fully anonymised secondary data. No human participants
were recruited and no personal or identifiable information is processed, which is why the
project fell under the EthOS self-assessment route.

|Dataset|Records|Positive rate|Role|
|-|-|-|-|
|Survey Lung Cancer|309|87.4%|Training and held-out testing|
|Cancer Patients and Air Pollution|1,000|36.5%|External validation (11 shared features)|

The datasets are not redistributed here beyond what is required for assessment. They originate
from public repositories and are included so that the pipeline can be re-executed exactly.

\---

## Reproducing the analysis

### Option 1 — Google Colab (matches the original environment)

The notebook was developed and executed in Google Colab. Paths inside it point to
`/content/sample\_data/`.

1. Open the notebook in Colab
2. Upload both CSV files into `/content/sample\_data/`
3. Run all cells in order (Runtime → Run all)

### Option 2 — Local environment

```bash
git clone <repository-url>
cd <repository-name>
python -m venv venv
source venv/bin/activate          # Windows: venv\\Scripts\\activate
pip install -r requirements.txt
jupyter notebook lung\_cancer\_dissertation\_final.ipynb
```

Running locally requires editing the two `pd.read\_csv()` paths near the top of the notebook to
point at the `data/` directory instead of `/content/sample\_data/`.

### Determinism

A single `RANDOM\_SEED` constant governs every stochastic operation. All 40 `random\_state`
arguments in the notebook reference it, so re-execution reproduces the reported statistical
results exactly. This was verified by re-running the full pipeline on different hardware:
every cross-validation estimate, bootstrap interval and significance test reproduced to the
last reported decimal.

The one exception is the carbon measurement. CodeCarbon estimates processor power from the
reported thermal design profile, so energy and emissions figures vary with the machine and
grid region the notebook happens to run on. They characterise a particular runtime rather than
the analysis itself.

\---

## Environment

Verified versions from the executed notebook:

|Package|Version|
|-|-|
|Python|3.13.15|
|NumPy|2.1.3|
|pandas|2.2.3|
|scikit-learn|1.6.1|
|XGBoost|3.4.1|

Also required: `matplotlib`, `scipy`, `shap`, `statsmodels`, `codecarbon`. See
`requirements.txt`.



## Notes on scope

* External validation is framed as **cross-benchmark transferability**, not epidemiological
external validation. Both datasets are convenience benchmarks rather than representative
population cohorts, and the dissertation treats this distinction as a stated limitation in
line with TRIPOD+AI reporting guidance.
* Attribution evidence (SHAP, permutation importance) is treated as corroboration rather than
proof. The two methods are computed on different estimators and are reported with that
difference disclosed.
* The models are research artefacts. They are not validated clinical tools and must not be
used to inform patient care.

