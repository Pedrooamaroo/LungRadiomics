# 🫁 PulmoRad-ML: 2D/3D Radiomic Lung Nodule Classification

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![PyRadiomics](https://img.shields.io/badge/PyRadiomics-Feature_Extraction-orange.svg)](https://pyradiomics.readthedocs.io/)
[![XGBoost](https://img.shields.io/badge/XGBoost-Ensemble_Learning-green.svg)](https://xgboost.readthedocs.io/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine_Learning-yellow.svg)](https://scikit-learn.org/)

## 🎯 Project Overview
Lung cancer remains a leading cause of mortality, making early detection via Computer-Aided Diagnosis (CAD) critical. This project implements a robust machine learning pipeline using the **LIDC-IDRI** dataset (1018 patients) to distinguish between **Benign** and **Malignant** pulmonary nodules.

Unlike standard CNN approaches, this project focuses on **Radiomics**—extracting quantifiable shape and texture features (2D and 3D) from CT scans—and rigorously testing feature selection strategies to maximize model explainability and performance.

---

## 📈 Executive Performance Summary
The study compared three classifiers (SVM, Random Forest, XGBoost) across four feature selection techniques (Lasso, PCA, t-test, correlation).

| Model Architecture | Feature Selection | Dimensionality | Test Accuracy | Recall (Sensitivity) |
| :--- | :--- | :--- | :--- | :--- |
| **XGBoost** | **t-test (p<0.005)** | **2D Features** | **~82%** | **0.91** |
| Random Forest | Lasso (L1) | 3D Features | ~79% | 0.88 |
| SVM (RBF) | PCA (95% Var) | 2D Features | ~75% | 0.84 |

**Key Takeaway:** While 3D volumetric features provide spatial depth, **2D features selected via statistical t-tests combined with XGBoost** yielded the most robust balance of Accuracy and Sensitivity (Recall)—a crucial metric in medical diagnosis to minimize false negatives.

---

## 🔬 Technical Deep Dive

### 1. Data Pipeline & Preprocessing
* **Annotation Clustering:** Used `pylidc` to aggregate annotations from 4 radiologists. Nodules with a malignancy score of 3 (indeterminate) were grouped with malignant cases (4-5) to adopt a conservative, high-sensitivity approach.
* **Hounsfield Unit (HU) Normalization:** CT scans were normalized to standard lung window ranges to ensure pixel intensity consistency.
* **Segmentation:** Generated dynamic masks for both 2D slices and 3D volumes to isolate nodule ROI (Region of Interest).

### 2. Feature Engineering (PyRadiomics)
Extracted **100+ radiomic features** per nodule, categorized into:
* **Shape:** Sphericity, Elongation, Surface Area.
* **First-Order Statistics:** Mean, Skewness, Kurtosis, Entropy.
* **Texture (GLCM/GLRLM):** Contrast, Correlation, Gray Level Non-Uniformity.

### 3. Feature Selection Strategy
To cure the "Curse of Dimensionality," I implemented independent selection pipelines:
* **Lasso (L1):** Penalized regression to zero-out irrelevant coefficients.
* **PCA:** Reduced dimensions while retaining 95% variance.
* **Statistical t-test:** Filtered features with $p < 0.005$ between classes.

---

## 📊 Visual Insights

| Feature Importance (XGBoost) | 3D Segmentation Mesh |
| :--- | :--- |
| <img src="images/feature_importance.png" width="400" alt="Feature Importance Plot"> | <img src="images/3d_mesh.png" width="400" alt="3D Nodule Mesh"> |

**Analysis:**
* **Spiculation** and **Lobulation** emerged as top predictors for malignancy, aligning with established radiological markers.
* **Calcification** showed a strong inverse correlation with malignancy (highly calcified nodules are often benign).

---

## 🛠️ Installation & Usage

### 1. Clone & Setup
```bash
git clone [https://github.com/YourUsername/PulmoRad-ML.git](https://github.com/YourUsername/PulmoRad-ML.git)
cd PulmoRad-ML
pip install -r requirements.txt
