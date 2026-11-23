# Relativistic Redshift & Celestial Classification

An astrophysical machine learning pipeline built on **Sloan Digital Sky Survey (SDSS17)** data to classify Stars, Galaxies, and Quasars and estimate their relativistic recession velocities ($\beta = v/c$) using Einstein's Relativistic Doppler equation.

## Team & Task Split

| Member     | Branch       | Notebook                                     | Responsibility                                    |
| :--------- | :----------- | :------------------------------------------- | :------------------------------------------------ |
| **Arya**   | `dev-arya`   | `notebooks/01_relativity_and_cleaning.ipynb` | SymPy Doppler math + SDSS17 cleaning              |
| **Sakshi** | `dev-sakshi` | `notebooks/02_spectral_pca.ipynb`            | 5D light vectors + PCA/SVD to 2D                  |
| **Ritu**   | `dev-ritu`   | `notebooks/03_kmeans_clustering.ipynb`       | K-Means ($k=3$) clustering & evaluation           |
| **Somil**  | `dev-somil`  | `notebooks/04_redshift_regression.ipynb`     | Linear Regression + velocity ($\beta$) estimation |

## Quick Setup

```bash
pip install -r requirements.txt
```
