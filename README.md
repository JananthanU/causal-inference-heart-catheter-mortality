# Causal Inference: Right Heart Catheterization and Mortality

Does right heart catheterization (RHC) in the first 24 hours of intensive care raise 30-day mortality? This project answers that with causal machine learning on the SUPPORT data analysed by Connors et al. (1996), and asks how strong unmeasured confounding would have to be to explain why the randomized trials found no effect.

`Python` `Causal Inference` `Causal Forest` `EconML` `AIPW` `IPW` `Sensitivity Analysis`

> **Status: work in progress.** The repository structure, setup and data loading are in place. The analysis follows the eight steps below.

---

## Why this question

Connors et al. (1996) found higher 30-day mortality among patients who received RHC (odds ratio 1.24, 95% CI 1.03 to 1.49). An editorial called for a moratorium, and RHC use in the US fell by about two thirds between 1993 and 2004. A later meta-analysis of 13 randomized trials found no effect on mortality (odds ratio 1.04, 95% CI 0.90 to 1.20). Was the observational estimate real harm, or residual confounding by severity of illness?

## Approach

The notebook follows the same eight-step causal ML pipeline as the [smoking cessation project](https://github.com/JananthanU/causal-inference-smoking-weight).

| Step | Plan |
|---|---|
| **1. Research question** | ATE of RHC on 30-day mortality (risk difference in percentage points); sensitivity against the randomized evidence and CATE as secondary questions. 5,735 patients, 2,184 with RHC |
| **2. Causal structure** | DAG with day-1 confounders (diagnosis, severity scores, physiology, comorbidities, demographics) |
| **3. Estimand** | ATE primary; overlap population as an alternative, since some patients almost always or never receive RHC |
| **4. Assumptions** | SUTVA, positivity (overlap and trimming), balance after weighting |
| **5. Fit** | Nuisance models, causal forest, estimator ladder from naive to AIPW, compared with Connors et al. and Hirano and Imbens (2001) |
| **6. Evaluate** | Calibration test, predicted vs. confirmed effects, RATE |
| **7. Robustness** | Placebo treatment, random common cause, semi-synthetic outcome, sensitivity to unmeasured confounding benchmarked against the randomized trials |
| **8. Interpretation** | Best linear projection, partial dependence, illustrative patient profiles |

## Data

The RHC dataset from the SUPPORT study (Study to Understand Prognoses and Preferences for Outcomes and Risks of Treatments), published by [Vanderbilt Biostatistics](https://hbiostat.org/data/) for methodological work on causal inference. The notebook downloads it on the first run and checks it against a SHA-256 checksum. The data are not stored in this repository.

## Reproducibility

```bash
git clone https://github.com/JananthanU/causal-inference-heart-catheter-mortality
cd causal-inference-heart-catheter-mortality
pip install -r requirements.txt
jupyter notebook notebooks/rhc_causal_ml_pipeline.ipynb
```

Run **Kernel > Restart Kernel and Run All Cells**. One seed (42) controls all random steps.

## References

- Connors AF et al. (1996). The effectiveness of right heart catheterization in the initial care of critically ill patients. *JAMA* 276:889–897.
- Hirano K, Imbens GW (2001). Estimation of causal effects using propensity score weighting: an application to data on right heart catheterization. *Health Services and Outcomes Research Methodology* 2:259–278.
- Shah MR et al. (2005). Impact of the pulmonary artery catheter in critically ill patients: meta-analysis of randomized clinical trials. *JAMA* 294:1664–1670.
- Wiener RS, Welch HG (2007). Trends in the use of the pulmonary artery catheter in the United States, 1993–2004. *JAMA* 298:423–429.
