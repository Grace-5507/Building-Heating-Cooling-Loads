# Building-Heating-Cooling-Loads
This study looked into assessing the heating load and cooling load requirements of buildings (that is, energy efficiency) as a function of building parameters.




# **Results & Discussion**

## 1. Problem Definition

The goal of this project is to **predict Heating Load (Y1) and Cooling Load (Y2)** of buildings using the **ENB2012 dataset**, which contains 768 samples with 8 design-related input features (e.g., wall area, glazing area, orientation, etc.). Accurate prediction of these loads is important for improving **energy efficiency** in building design.



## 2. Data Understanding & Wrangling

* **Shape:** 768 rows × 10 columns.
* **Features (X1–X8):** Numeric design parameters (continuous + integer).
* **Targets:** Heating Load (Y1), Cooling Load (Y2).
* **Preprocessing:**

  * Checked for missing values (none found).
  * Outlier detection using boxplots (extreme values present but within valid ranges).
  * Encoding/scaling: Not required since all features are numeric.
* **EDA:**

  * Histograms showed varying distributions (some symmetric, others skewed).
  * Correlation analysis revealed strong correlations between surface area and energy loads.
  * Boxplots indicated wider variability in Cooling Load compared to Heating Load.



## 3. Baseline

Before modeling, we established a **baseline model** using the mean of target values as predictions.

* **Baseline MAE:** \~9.1
  This represents the error if we made predictions without learning any relationships.



## 4. Modeling

We tested several models:

| Model                   | Train MAE | Val MAE | Notes                                          |
| ----------------------- | --------- | ------- | ---------------------------------------------- |
| Decision Tree (depth=5) | 0.595     | 0.701   | Simple, interpretable, some overfitting        |
| Random Forest           | 0.312     | 0.401   | Better generalization, still not best          |
| Linear Regression       | 2.088     | 1.952   | Poor performance → relationships are nonlinear |
| Gradient Boosting       | 0.235     | 0.313   | Strongest baseline performer                   |

**Interpretation:**
Tree-based models (Random Forest, Gradient Boosting) clearly outperform Linear Regression, showing that the problem is **nonlinear**. Gradient Boosting already shows excellent performance.



## 5. Hyperparameter Tuning & Validation

We tuned **Gradient Boosting** with GridSearchCV and RandomizedSearchCV, testing combinations of `n_estimators`, `learning_rate`, and `max_depth`.

* **Best Parameters:**

  ```
  n_estimators = 300  
  learning_rate = 0.1  
  max_depth = 4  
  ```

* **Best Validation MAE:** 0.25

* **Cross-validation result:** Mean CV MAE = **0.25 ± 0.02**
  → Shows the model generalizes well beyond one lucky train/val split.



## 6. Residual Analysis

* **Residual distribution:** Approximately bell-shaped around zero → indicates errors are unbiased.
* **Residuals vs Predictions:** Scattered around zero → no clear pattern, suggesting the model captured the main structure of the data without systemic bias.



## 7. Feature Importance

Gradient Boosting revealed that:

* **Wall Area, Roof Area, and Overall Height** were the most influential predictors.
* **Orientation** had relatively less impact.



 ##8. Conclusion

* The **Tuned Gradient Boosting Regressor** achieved the **lowest MAE (0.25)**, a dramatic improvement over the baseline (9.1).
* Linear models underperformed, confirming the necessity of nonlinear approaches.
* Residual analysis and cross-validation confirm the model is reliable and generalizes well.
* For deployment, Gradient Boosting should be the **primary model**, while simpler models like Decision Tree could be used for **interpretability**.

