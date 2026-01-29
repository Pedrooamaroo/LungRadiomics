# 🫁 LungRadiomics: Radiomic Lung Nodule Classification

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![LIDC-IDRI](https://img.shields.io/badge/Dataset-LIDC--IDRI-lightgrey)](https://www.cancerimagingarchive.net/collection/lidc-idri/)
[![XGBoost](https://img.shields.io/badge/Model-XGBoost-green)](https://xgboost.readthedocs.io/)

## 📋 Project Overview
**LungRadiomics** is a machine learning pipeline designed to assist in the early diagnosis of lung cancer. Unlike traditional Deep Learning approaches that operate as "black boxes," this project leverages **Radiomics**, the extraction of quantifiable mathematical features (shape, texture, intensity) from medical images, to classify pulmonary nodules as **Benign** or **Malignant**.

The system processes **CT Scans**, segments nodules in 2D/3D space, and applies statistical feature selection to train interpretable classifiers.

---

## 🏆 Key Results (Executive Summary)
The model was evaluated using accuracy and **Recall (Sensitivity)**, identifying Malignancy with high confidence to minimize False Negatives in a clinical setting.

| Model | Feature Selection | Dimensionality | **Accuracy** | **Recall (Sensitivity)** |
| :--- | :--- | :--- | :--- | :--- |
| **XGBoost** | **t-test (p < 0.005)** | **2D Radiomics** | **82%** | **0.91** |
| Random Forest | Lasso (L1) | 3D Radiomics | 79% | 0.88 |
| SVM (RBF) | PCA (95% Var) | 2D Radiomics | 75% | 0.84 |

> **Clinical Insight:** The XGBoost model achieved the highest sensitivity (0.91), meaning it successfully identified 91% of malignant cases, making it the most viable candidate for a medical screening support tool.

---

## 📂 Dataset & Access
This project utilizes the **LIDC-IDRI (Lung Image Database Consortium)** dataset, consisting of diagnostic and lung cancer screening thoracic CT scans with marked-up annotated lesions.

* **Source:** The Cancer Imaging Archive (TCIA)
* **Collection:** [LIDC-IDRI Collection URL](https://www.cancerimagingarchive.net/collection/lidc-idri/)
* **Size:** ~1018 patients (120GB+)
* **License:** Creative Commons Attribution 3.0 Unported

**⚠️ Note:** Due to the size and licensing of the dataset, the raw DICOM images are **not** included in this repository. You must download the dataset using the NBIA Data Retriever or the `pylidc` library API.

---

## ⚙️ Methodology & Pipeline

The project follows the **CRISP-DM** methodology across four stages:

1.  **Data Ingestion & Cleaning:**
    * Parsing DICOM metadata using `pylidc`.
    * Handling missing values and filtering patients with 0 nodules.
    * **Risk Mapping:** Indeterminate malignancy scores (3/5) were mapped to "Malignant" to prioritize patient safety.
2.  **Preprocessing:**
    * Conversion to **Hounsfield Units (HU)** for density standardization.
    * 2D and 3D Segmentation of nodule volumes.
3.  **Feature Extraction (Radiomics):**
    * Extraction of Shape (Sphericity, Compactness), Texture (GLCM, GLRLM), and Intensity (Histogram) features.
4.  **Modeling:**
    * Comparison of SVM, Random Forest, and XGBoost.
    * Hyperparameter tuning via Grid Search.

---

## 💻 Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/pedrooamaroo/PulmoRad-ML.git](https://github.com/pedrooamaroo/PulmoRad-ML.git)
    cd PulmoRad-ML
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Run the Notebook:**
    Launch Jupyter to explore the analysis pipeline:
    ```bash
    jupyter notebook "Lung_Cancer_Radiomics.ipynb"
    ```
