# Predicting Popular Recipes for Tasty Bytes 🍽️

## Project Overview

This project is the **final practical exam** for the **DataCamp Data Scientist Certification**.  
The objective is to help **Tasty Bytes**, a recipe website, identify which recipes should be featured on the homepage in order to **maximize website traffic**.

Specifically, the task is to build a **classification model** that predicts whether a recipe will generate **high traffic**, with a business requirement that the model should correctly identify high-traffic recipes **at least 80% of the time**.

The project follows the full data science workflow:
- Data validation and cleaning  
- Exploratory data analysis  
- Model development and evaluation  
- Business-oriented recommendations  

---

## Business Problem

The product team at Tasty Bytes wants to improve homepage performance by only featuring recipes that are likely to attract high user traffic.

**Key business constraint:**
- Missing a high-traffic recipe is costly  
- Therefore, **recall for high-traffic recipes is prioritized** over overall accuracy

This framing directly influences model selection, evaluation metrics, and threshold tuning.

---

## Dataset Description

Each row represents a recipe published on the website.

### Key Features
- `calories`
- `carbohydrate`
- `sugar`
- `protein`
- `category`
- `servings`

### Target Variable
- `high_traffic`  
  - `High` → recipe generated high traffic  
  - Missing values are treated as `Low`

---

## Data Validation & Cleaning

Several data quality issues were identified and addressed:

### 1. Missing Numerical Values
- Columns affected: `calories`, `carbohydrate`, `sugar`, `protein`
- Each contained ~52 missing values
- **Solution**: Median imputation using `SimpleImputer`
  - Median is robust to outliers present in nutritional data

### 2. Target Variable Cleaning
- `high_traffic` had a large number of missing values
- **Assumption**: Missing values imply the recipe did not generate high traffic
- Missing values were filled with `"Low"` and converted to binary:
  - `High → 1`
  - `Low → 0`

> ⚠️ This assumption may introduce label noise and is discussed as a limitation.

### 3. `servings` Formatting Issue
- Values included text (e.g. `"4 as a snack"`)
- **Solution**: Regular expressions used to extract numeric values

### 4. Category Inconsistencies
- Example: `"Chicken Breast"` vs `"Chicken"`
- Categories were standardized prior to one-hot encoding

---

## Exploratory Data Analysis (EDA)

EDA was conducted to understand distributions, relationships, and potential modeling challenges.

### Univariate Analysis
- Nutritional variables are **right-skewed** with long tails
- Several extreme values are present
- Category distribution is imbalanced (e.g. Breakfast and Beverages dominate)

### Multivariate Analysis
- Scatter plots (e.g. sugar vs calories, colored by category) show:
  - No strong linear relationships
  - Clear differences between recipe categories

**Insight:**  
Non-linearity exists, but a linear model can still serve as a strong, interpretable baseline.

---

## Modeling Approach

### Problem Type
- Binary classification (High Traffic vs Low Traffic)

### Models Evaluated

#### 1. Logistic Regression (Baseline Model)
- Chosen for interpretability and stability
- `max_iter = 50,000`
- Outputs calibrated probabilities, useful for threshold tuning

#### 2. Random Forest (Challenger Model)
- Captures non-linear relationships
- `n_estimators = 300`
- `class_weight = "balanced"`

---

## Model Evaluation

Given business priorities, **recall for high-traffic recipes** is the primary metric.

| Model              | Accuracy | Recall (High) | F1 Score |
|--------------------|----------|---------------|----------|
| Logistic Regression| ~77%     | ~79%          | ~78%     |
| Random Forest      | ~73%     | ~76%          | ~74%     |

### Threshold Optimization
- Logistic Regression probabilities were used to adjust the classification threshold
- Lowering the threshold from **0.50 → 0.43** increased recall for high-traffic recipes
- This trade-off aligns with business goals by reducing false negatives

---

## Final Model Selection

✅ **Logistic Regression** was selected as the final model because:
- It outperformed Random Forest on key business metrics
- It is interpretable and easier to explain to stakeholders
- Threshold tuning allows flexible control over recall vs precision

**Final performance:**  
- High-traffic recall ≈ **79%**, close to the 80% business target

---

## Business Metrics to Monitor

If deployed, the following metrics should be tracked:

### Primary Metric
- **Recall (High Traffic Recipes)**  

### Secondary Metrics
- Precision for high-traffic predictions  
- Homepage traffic uplift (A/B testing)
- Conversion to paid subscriptions (if available)

---

## Recommendations & Next Steps

1. **A/B Testing**
2. **Feature Expansion**
3. **Advanced Models**
4. **Ongoing Monitoring**

---

## Limitations

- Treating missing `high_traffic` values as `Low` may introduce label noise
- Dataset lacks user behavior and temporal features
- Class imbalance may affect long-term model stability

---

## Repository Structure

```
├── README.md
├── data/
│   └── recipe_site_traffic_2212.csv（not available）
├── notebook/
│   └── analysis.ipynb
├── presentation/
│   └── Predicting_Popular_Recipes_for_Tasty_Bytes.pdf
```

---

## Tools & Libraries
- Python
- pandas
- scikit-learn
- matplotlib / seaborn
- Jupyter Notebook

---

## Conclusion

This project demonstrates a complete, end-to-end data science workflow aligned with real business objectives.  
By prioritizing recall and model interpretability, the final solution delivers actionable insights while remaining practical for deployment.

---

*Completed as part of the DataCamp Data Scientist Certification Practical Exam.*
