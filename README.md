# README

## Overview
This repository contains an analysis aimed at identifying key genes associated with treatment response in an autoimmune disease and building a predictive model to classify responders and non-responders. The data consists of:

- **Metadata** with patient clinical information (e.g., disease activity score, response status, gender, etc.).  
- **Gene Expression** data with thousands of genes measured for each patient.

## 1. Exploratory Data Analysis (EDA)
- **Missing Values:**
  - The metadata had missing values in both the disease activity score (das28) and response status columns. We dropped rows with missing response status (our target variable) to ensure complete cases for the modeling phase.
  - Gene expression data also had some missing values, which were handled by imputation or by dropping as needed.
- **Distributions and Outliers:**
  - Many of the gene expression columns were skewed. A log transform (`log1p`) was applied to normalize their distributions before applying statistical tests or modeling.
  - The disease activity score (das28) displayed a moderate range of values, with some outliers. These outliers were retained, assuming they reflected real clinical variation.

## 2. Feature Selection
- **Methodology:**
  - A Welch’s t‑test (unequal variance t‑test) was used to compare gene expression between responders and non-responders. Before the test, a log transform was applied to gene expression values to better meet normality assumptions.
  - Genes were ranked by p-value (lowest to highest).
- **Top Genes:**
  - The top 10 genes (by smallest p-value) showed significant expression differences between responders and non-responders.
  - These genes are strong candidates for further study or targeted biomarker development.

## 3. Predictive Modeling
- **Models Compared:**
  1. **Baseline Model:** Logistic Regression  
  2. **Sophisticated Model:** Random Forest (and, in some experiments, Support Vector Classifier or XGBoost)

- **Data Preparation:**
  - **Dimensionality Reduction:** When using the entire dataset, Principal Component Analysis (PCA) was applied to reduce thousands of gene features to a manageable number of components (e.g., 10). These components were then combined with clinical metadata.
  - **Train/Test Split:** An 80/20 split was used for training and evaluation.

- **Performance Metrics:**
  - **Accuracy** (overall correctness)  
  - **Precision** (fraction of predicted positives that are true positives)  
  - **Recall** (fraction of actual positives identified correctly)  
  - **F1 Score** (harmonic mean of precision and recall)  
  - **AUC-ROC** (discriminative ability across thresholds)

- **Model Results & Interpretation:**
  - The **Logistic Regression** model performed surprisingly well, often surpassing more complex models (e.g., Random Forest) on this dataset. It achieved decent recall (minimizing missed responders) and acceptable precision.
  - **Random Forest** sometimes showed lower AUC-ROC and accuracy than logistic regression, possibly due to the small sample size and high-dimensional feature space.
  - In scenarios where the top 10 genes were used, the performance was often comparable or slightly better than when all genes were included, suggesting that focusing on the most relevant genes can reduce noise and improve interpretability.

## 4. Explainability and Feature Importance
- **SHAP Analysis:**
  - For the logistic regression model, SHAP values indicated that certain PCA components and clinical metadata (e.g., disease activity score) had the greatest impact on classification.
- **Permutation Importance:**
  - Permutation importance also confirmed that disease activity score (das28) and select PCA components contributed most to model accuracy.
- **Interpretation in Clinical Context:**
  - Clinical severity (das28) consistently emerged as a key predictor of response status.
  - Specific PCA components—reflecting underlying gene expression patterns—also played a major role in distinguishing responders from non-responders. Additional analysis of the gene loadings revealed potential pathways and genes of interest.

## Conclusion
This analysis underscores the value of combining both clinical data and gene expression in predicting treatment response. Despite a relatively small dataset and high dimensionality, logistic regression performed robustly, particularly after feature selection or PCA. Further validation on larger cohorts and additional functional studies on the top-ranked genes are recommended to confirm their role in treatment response.
