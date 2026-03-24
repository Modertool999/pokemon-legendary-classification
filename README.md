<p align="center">
  <img src="assets/pokeball_dash.gif" alt="Pokeball dash gif" width="420">
</p>

<h1 align="center">Pokémon Legendary Classification</h1>

<p align="center">
  Predicting whether a Pokémon is legendary using gameplay and stat features.
</p>

## Overview

This project started as a fun question I had while looking through a full Pokémon dataset:

**Can you tell whether a Pokémon is legendary just from a small set of numeric gameplay/stat features?**

I trained and compared a couple of classic ML models on a cleaned version of the dataset and ended up with two pretty interesting takeaways:

- even a simple **logistic regression** model performs extremely well
- a **random forest** model is almost perfect on this task and stays that way under cross-validation

What made the project especially fun was that the most important features were not just raw battle stats. Game-design metadata like **capture rate**, **base egg steps**, and **experience growth** turned out to be some of the strongest signals.

## Dataset

- Source: Kaggle Pokémon dataset scraped from Serebii
- Rows: 801 Pokémon
- Target: `is_legendary`
- Final feature subset used in the main models:
  - `attack`
  - `base_egg_steps`
  - `base_happiness`
  - `capture_rate`
  - `defense`
  - `experience_growth`
  - `hp`
  - `sp_attack`
  - `sp_defense`
  - `speed`
  - `weight_kg`

## What I built

I trained and evaluated:

- **Logistic Regression**
- **Random Forest**

To make the comparison more reliable, I used:

- median imputation for missing numeric values
- feature scaling for logistic regression
- **stratified 5-fold cross-validation**
- class-imbalance-aware evaluation metrics:
  - accuracy
  - precision
  - recall
  - F1
  - ROC-AUC

## Results

### Cross-validated model performance

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.9651 | 0.7466 | 0.9286 | 0.8260 | 0.9939 |
| Random Forest | **0.9963** | **0.9600** | **1.0000** | **0.9793** | **0.9996** |

<p align="center">
  <img src="assets/cv_model_comparison.png" alt="Cross-validated model comparison" width="780">
</p>

On a held-out test split, logistic regression reached **98.1% accuracy** and the random forest reached **100% accuracy**. I was a little more careful about the random forest result in the writeup, since one perfect split matters less than performance across multiple folds. The cross-validation numbers ended up telling the more useful story anyway.

## Interpretation

One of my goals was to make the project more than just “fit a model and report accuracy,” so I also looked at **model interpretability** from two angles:

- logistic regression **coefficients**
- random forest **feature importances**

### Logistic regression coefficients

<p align="center">
  <img src="assets/logreg_coefficients.png" alt="Logistic regression coefficients" width="760">
</p>

### Random forest feature importances

<p align="center">
  <img src="assets/rf_importances.png" alt="Random forest feature importances" width="760">
</p>

Both models pointed to the same small set of standout predictors:

- **base_egg_steps**
- **capture_rate**
- **experience_growth**

That was probably the most interesting part of the project for me. The model was not just learning that legendary Pokémon tend to have strong stats; it was also picking up on how the games themselves treat legendary Pokémon as special cases.

## Key takeaways

- A small set of numeric features is enough to separate legendary from non-legendary Pokémon extremely well.
- **Random forest** clearly outperformed a strong logistic regression baseline, which suggests some nonlinear structure in the data.
- The most informative features were tied to **rarity and progression mechanics**, not just battle strength.

## Tech stack

- Python
- pandas
- NumPy
- scikit-learn
- Matplotlib
- Jupyter Notebook

## Repo structure

```text
.
├── pokemon_cleaned.ipynb
├── README.md
└── assets
    ├── pokeball_dash.gif
    ├── cv_model_comparison.png
    ├── logreg_coefficients.png
    └── rf_importances.png
```

## How to run

1. Install the Python dependencies you need (`pandas`, `numpy`, `scikit-learn`, `matplotlib`, `kagglehub`).
2. Open the notebook.
3. Download/load the dataset.
4. Run the cells in order to reproduce the preprocessing, training, evaluation, and plots.

## Resume-friendly summary

- Built and evaluated ML models to predict Pokémon legendary status from gameplay and stat data, achieving **99.6% cross-validated accuracy** and **0.9996 ROC-AUC** with a random forest classifier.
- Compared linear and nonlinear approaches using **stratified 5-fold cross-validation** and class-imbalance metrics, showing ensemble models significantly outperformed a strong logistic regression baseline.
- Interpreted model behavior with coefficient and feature-importance analysis, identifying **capture rate, base egg steps, and experience growth** as the strongest predictors.
