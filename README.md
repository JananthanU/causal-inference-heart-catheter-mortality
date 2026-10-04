# Causal Inference: Right Heart Catheterization and Mortality

Does a right heart catheter on the first day of intensive care raise 30-day mortality? This project estimates the effect with causal machine learning on the SUPPORT data that Connors et al. analysed in 1996. It also asks how much unmeasured confounding it would take to explain why randomized trials later found no effect.

`Python` `Causal Inference` `Causal Forest` `EconML` `AIPW` `IPW` `Sensitivity Analysis`

Work in progress. Setup and data loading are done, the analysis follows the eight steps below.

---

## Background

Connors et al. found higher 30-day mortality among patients who received a catheter. An editorial called for a moratorium, and catheter use in the US fell by about two thirds between 1993 and 2004. Randomized trials, pooled in a meta-analysis in 2005, found no effect on mortality. Was the observational result real harm, or confounding by severity of illness?

| Study | Design | Effect on mortality |
|---|---|---|
| Connors et al. 1996 | Observational, 5,735 patients | OR 1.24, 95% CI 1.03 to 1.49 |
| Shah et al. 2005 | Meta-analysis of 13 RCTs, 5,051 patients | OR 1.04, 95% CI 0.90 to 1.20 |

## Approach

The notebook follows the same eight-step pipeline as my [smoking cessation project](https://github.com/JananthanU/causal-inference-smoking-weight).

| Step | Plan |
|---|---|
| **1. Research question** | Effect of the catheter on 30-day mortality as a risk difference in percentage points. Sensitivity against the randomized evidence and effect heterogeneity as secondary questions |
| **2. Causal structure** | DAG with the day-1 confounders: diagnosis, severity scores, physiology, comorbidities and demographics |
| **3. Estimand** | ATE as primary estimand. Some patients almost always or almost never get a catheter, so the overlap population serves as an alternative |
| **4. Assumptions** | SUTVA, overlap and trimming, balance after weighting |
| **5. Estimation** | Nuisance models, causal forest and an estimator ladder from the naive difference to AIPW, compared with Connors et al. and Hirano and Imbens 2001 |
| **6. Evaluation** | Calibration test, predicted vs. confirmed effects, RATE |
| **7. Robustness** | Placebo treatment, random common cause, semi-synthetic outcome, sensitivity to unmeasured confounding benchmarked against the randomized trials |
| **8. Interpretation** | Best linear projection, partial dependence, example patient profiles |

## Data

The RHC data from the SUPPORT study are published by [Vanderbilt Biostatistics](https://hbiostat.org/data/) for methods research on causal inference. The notebook downloads them on the first run and verifies a SHA-256 checksum. The data are not stored in this repository.

## Reproducibility

```bash
git clone https://github.com/JananthanU/causal-inference-heart-catheter-mortality
cd causal-inference-heart-catheter-mortality
pip install -r requirements.txt
jupyter notebook notebooks/rhc_causal_ml_pipeline.ipynb
```

Run **Kernel > Restart Kernel and Run All Cells**. One seed, 42, controls all random steps.

## References

- Connors AF, Speroff T, Dawson NV, et al. The effectiveness of right heart catheterization in the initial care of critically ill patients. JAMA 1996;276:889–897.
- Hirano K, Imbens GW. Estimation of causal effects using propensity score weighting: an application to data on right heart catheterization. Health Services and Outcomes Research Methodology 2001;2:259–278.
- Shah MR, Hasselblad V, Stevenson LW, et al. Impact of the pulmonary artery catheter in critically ill patients: meta-analysis of randomized clinical trials. JAMA 2005;294:1664–1670.
- Wiener RS, Welch HG. Trends in the use of the pulmonary artery catheter in the United States, 1993–2004. JAMA 2007;298:423–429.
