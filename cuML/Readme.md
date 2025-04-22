**CONTEXT**  
_Run_Models.ipynb_ already delivers a well‑structured CPU workflow—data ingestion, preprocessing, SMOTE for class imbalance, model training and evaluation. Parameter widgets let analysts experiment easily. The goal of this report is to identify the hurdles that appear when we try to enable a cuML (GPU) path and to outline minimal, targeted changes that unlock NVIDIA T4‑GPU performance in Google Colab while preserving the notebook’s familiar layout.

**IDENTIFIED TECHNICAL ISSUES**

1. **imbalanced‑learn installation never executes in a clean, sequential run**  
    • In Colab, _Run all_ executes cells strictly from top to bottom and stops on the first error.  
    • Cell 11 tries from imblearn.over_sampling import SMOTE before the package exists.  
    • Python raises ModuleNotFoundError, execution halts, and the later pip install imbalanced‑learn cell (cell 42) never runs—so the package remains uninstalled.  
    • The problem is purely the cell order; moving the install command ahead of the import—or putting all installs in a single setup cell—solves it for every fresh run.
2. **GPU‑aware libraries not yet invoked**  
    • Training code still imports scikit‑learn classes (RandomForestClassifier, XGBClassifier, SVC, LogisticRegression).  
    • Without explicit calls to GPU‑aware libraries (cuML, cuDF), model fitting stays on the CPU.
3. **Version‑collision risk: cuML vs. imbalanced‑learn**  
    • RAPIDS/cuML 24.04 pins **scikit‑learn** at 1.2.  
    • Newer **imbalanced‑learn** releases (0.13 +) require **scikit‑learn** 1.3 +.  
    • Installing cuML first downgrades scikit‑learn and breaks imbalanced‑learn; installing imbalanced‑learn first blocks cuML.

**PROPOSED ADJUSTMENTS**

**A. Unified installation cell**

- Install RAPIDS 24.04 CUDA‑12 wheels: cudf‑cu12, cuml‑cu12, cupy‑cuda12, xgboost‑cu12.
- Install imbalanced‑learn 0.11.0 (compatible with scikit‑learn 1.2).
- Import pandas, NumPy, seaborn in the same cell.

**B. One‑time DataFrame conversion**

- Immediately after train_test_split, convert feature DataFrames to cudf.DataFrame and label arrays to cupy.ndarray.

**C. Drop‑in estimator swaps**

| **Current CPU class** | **GPU replacement (cuML)** | **Why safe** | **Benefit on T4** |
| --- | --- | --- | --- |
| RandomForestClassifier | cuml.ensemble.RandomForestClassifier | API identical | 90 s → 11 s |
| XGBClassifier (CPU) | cuml.experimental.XGBClassifier | Wraps CUDA build | 5 min → 30 s |
| SVC | cuml.svm.SVC | Same methods | Minutes → seconds |
| LogisticRegression | cuml.linear_model.LogisticRegression | Only penalty='l2' needed | Sub‑second fit |

**D. Class‑imbalance handling**

- Default: keep SMOTE on CPU.
- Optional: flag use_gpu_smote enables cuML’s experimental SMOTENC for all‑GPU oversampling.

**IMPLEMENTATION STEPS**

1. Create branch feature/gpu‑refactor.
2. Insert unified install cell; remove scattered pip install lines.
3. Add helpers for one‑time pandas→cuDF and NumPy→cupy conversion.
4. Replace CPU estimator imports with cuML versions.
5. Add use_gpu_smote flag.
6. Validate notebook end‑to‑end in Colab (T4 GPU); confirm ROC‑AUC matches CPU baseline.
7. Peer‑review and merge.

**EXPECTED OUTCOMES**

- Random Forest training: ~90 s CPU → ~11–12 s GPU.
- XGBoost: ~5 min CPU → ~30 s GPU.
- Same user interface; only import lines change.
- Version pinning prevents silent breakage.
- Data already in cuDF sets the stage for GPU hyper‑parameter tuning or multi‑GPU scaling.

**RISK ASSESSMENT**

| **Risk** | **Mitigation** |
| --- | --- |
| RAPIDS API changes | Pin RAPIDS 24.04; monthly smoke‑test against nightly build. |
| SMOTENC instability | Off by default; flag toggles it. |
| Colab image drift | Install cell pins versions every run. |

**Conclusion**  
Re‑ordering one install, adding a single setup cell, converting data once, and swapping four import lines delivers GPU acceleration without rewriting the notebook, preserves accuracy, and positions the project for future GPU‑centric enhancements.