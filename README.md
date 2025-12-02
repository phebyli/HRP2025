# Early-Post-Op-HCC-Recurrence-Prediction  
&gt; A multimodal radiomics-clinicopathology framework for **early post-operative recurrence prediction** in **hepatocellular carcinoma (HCC)** after curative resection.

---

## Overview  
Accurate prediction of early recurrence (≤ 730 days) is critical for personalised surveillance and adjuvant therapy in HCC.  
We propose an **unsupervised GMM-filtered radiomics + multimodal fusion** pipeline that:  
1. Discovers **intrinsic CT imaging phenotypes** via Gaussian-Mixture clustering (no labels needed).  
2. Integrates **clinical (15 vars) + pathological (7 vars) + radiomics (top-50 distinctive features)**.  
3. Delivers **robust, interpretable models** through repeated nested cross-validation & SHAP explainability.  

**Key performance** (internal cohort, n = 142):  
AUC = **0.909** (95 % CI 0.900–0.918), Sensitivity = 0.831, Specificity = 0.858, outperforming any single-modality model (p &lt; 0.0001).

---

##  Framework Architecture  
![An Overview figure of early post-operative recurrence prediction.](overview.jpg)

| Module | Function |
| --- | --- |
| **STEP1_Radiomics** | ICC-based reproducibility filtering → 3 864 → **2 371** features. |
| **STEP2_GMM** | UMAP↓10D → GMM clustering → ANOVA → **top-50** distinctive features. |
| **STEP3_Prognosis** | Nested CV (20×5×5) + FDR feature selection + **stability ≥ 60 %** → final model. |

---

## Repository Content  
```
HRP2025/
├── STEP1_Radiomics.ipynb          # Feature extraction & ICC filtering
├── STEP2_GMM.ipynb                # Unsupervised clustering & top-50 selection
├── STEP3_Prognosis.ipynb          # Nested-CV training, testing, SHAP
├── configs/
│   └── CT.yaml                    # PyRadiomics parameter file
├── figures/                       # Auto-saved PNGs (UMAP, heat-map, ROC, SHAP)
├── results/                       # CSV outputs (clusters, stable features, CV scores)
└── data/                          # Placeholder – keep structure below
```

---

## Quick Start 
1. Clone repo  
```bash
git clone https://github.com/YOUR-GITHUB/HRP2025.git
cd HRP2025
```
2. Organise your data
```
data/
├── clinical.csv          # rows=patients, cols=15 clinical vars
├── pathological.csv      # rows=patients, cols=7 patho vars
├── recurrence.csv        # rows=patients, col1=0/1 (730-day recurrence)
├── imagelabel_1/         # patient folders with *-AP-image.nrrd & *-AP-label.nrrd
└── imagelabel_2/         # second observer (for ICC)
```
3. Run notebooks sequentially
```bash
jupyter nbconvert --to notebook --execute STEP1_Radiomics.ipynb
jupyter nbconvert --to notebook --execute STEP2_GMM.ipynb
jupyter nbconvert --to notebook --execute STEP3_Prognosis.ipynb
```
4. Results pop-up automatically
* ./results/nested_cv_results.csv – AUC, 95 % CI, stable features
* ./figures/* – UMAP, heat-map, ROC, SHAP bar/beeswarm plots

---

## Environment
Python ≥ 3.8
```bash
conda create -n hcc python=3.8 -y
conda activate hcc
pip install -r requirements.txt
```
---

## Citation
If you use this code, please cite:
Li Y, Wang Z, He Z, Guo C, Liu R, Rong P. Early post-operative HCC recurrence prediction using GMM-filtered radiomics and multimodal feature fusion.