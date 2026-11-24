Relativistic Redshift Estimation & Celestial Classification

An astrophysical linear algebra and machine learning pipeline built on ~100,000 real telescope observations from the **Sloan Digital Sky Survey (SDSS17)**.

> **Core Research Question:** Using only **5 raw photometric brightness readings** (`u, g, r, i, z`), can pure linear algebra and Special Relativity tell us both **what** a celestial object is (Star, Galaxy, or Quasar) and **how fast** it is receding from Earth ($\beta = v/c$)?

---
## Quick Setup

```bash
pip install -r requirements.txt
```

## How the 4-Phase Pipeline Works
Each phase lives in its own modular Jupyter Notebook and is orchestrated end-to-end by `notebooks/00_final_master_pipeline.ipynb`:

| Phase | Owner | Notebook | What It Does |
| :--- | :--- | :--- | :--- |
| **1. Relativity & Cleaning** | **Arya** | `notebooks/01_relativity_and_cleaning.ipynb` | Symbolically inverts Einstein's Relativistic Doppler equation in SymPy ($\beta(z)$ and $d\beta/dz$) and cleans corrupted `-9999` sensor outliers into `data/processed/sdss_clean.csv`. |
| **2. Spectral PCA / SVD** | **Sakshi** | `notebooks/02_spectral_pca.ipynb` | Standardizes the 5D light vectors (`u, g, r, i, z`), builds the $5 \times 5$ covariance matrix, verifies manual SVD against Scikit-Learn PCA, and projects into 2D (`data/processed/sdss_pca_2d.csv`). |
| **3. Cosmic Clustering** | **Ritu** | `notebooks/03_kmeans_clustering.ipynb` | Runs unsupervised K-Means ($k = 3$, `random_state=42`) on the 2D PCA coordinates without labels and evaluates cluster alignment against true cosmic classes. |
| **4. Redshift & Velocity** | **Somil** | `notebooks/04_redshift_regression.ipynb` | Trains Linear Regression on `u, g, r, i, z` (80/20 split, `random_state=42`) to predict redshift ($z$), then converts predictions into relativistic recession velocities ($\beta = v/c$). |

---

## Key Results & Honest Takeaways

### What Worked
* **97.4% Variance in 2D:** Compressing 5D telescope photometry down to 2 Principal Components retains **97.4% of total variance** (`PC1: 85.6%`, `PC2: 11.8%`), proving celestial light spectra occupy a structured low-dimensional subspace.
* **Relativity-Bounded Predictions:** Passing ML redshift predictions through Einstein's SymPy-derived Doppler equation ($\beta = \frac{(z+1)^2 - 1}{(z+1)^2 + 1}$) guarantees every estimated velocity strictly obeys Special Relativity ($\beta < 1.0c$).
* **Reproducible Engineering:** Fully modular 4-person workflow with relative paths, locked random seeds (`random_state=42`), 300 DPI figure exports in `figures/`, and a single master runner.

### Honest Limitations (Where Linear Models Hit Their Limit)
* **Cluster Overlap:** In 2D PCA space, Quasars form a tight band, but Stars and Galaxies naturally overlap in brightness—limiting how cleanly rigid, unsupervised K-Means boundaries can separate them.
* **Linear Regression Baseline:** Predicting redshift linearly from `u, g, r, i, z` achieves **$R^2 = 0.2789$ (`RMSE = 0.6210`)** for redshift and **$R^2 = 0.2016$ (`RMSE = 0.2384c`)** for velocity. Nearby **Stars** sit at $z \approx 0$ regardless of brightness, while distant **Quasars** ($z > 2$) behave non-linearly, flattening a linear hyperplane around $\hat{z} \approx 1.0$. This establishes a rigorous linear baseline and pinpoints where non-linear astrophysical modeling is required.

---

## Repository Structure
```text
relativistic-redshift-project/
├── data/
│   ├── raw/star_classification.csv        # Original SDSS17 CSV from Kaggle
│   └── processed/                         # Cleaned & PCA-transformed CSVs
├── figures/                               # Exported 300 DPI Matplotlib plots
├── notebooks/
│   ├── 00_final_master_pipeline.ipynb     # Master runner (executes Phases 1–4 in order)
│   ├── 01_relativity_and_cleaning.ipynb   # Phase 1 (Arya)
│   ├── 02_spectral_pca.ipynb              # Phase 2 (Sakshi)
│   ├── 03_kmeans_clustering.ipynb         # Phase 3 (Ritu)
│   └── 04_redshift_regression.ipynb       # Phase 4 (Somil)
├── requirements.txt                       # Core Python stack
└── README.md
```
