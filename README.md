# 🔬 Project Workflow

The project follows a structured machine learning workflow, starting with data quality and feature analysis and progressing toward feature selection and Random Forest regression.

```text
Raw Data
   │
   ▼
Data Understanding
   │
   ▼
Variance Analysis
   │
   ├── Remove 20 low-variance features
   │
   ▼
Correlation Analysis
   │
   ▼
Mutual Information
   │
   └── SelectKBest → 200 selected features
   │
   ▼
Random Forest Regression
   │
   ▼
Feature Importance Analysis
   │
   ▼
Feature Subset Comparison
   │
   ▼
Model Evaluation
```

---

## 1. 🔎 Data Understanding

The first stage focused on understanding the structure and characteristics of the dataset.

The analysis included:

* Inspecting the dataset dimensions
* Reviewing numerical features
* Examining feature variability
* Investigating relationships between variables
* Preparing the dataset for feature selection and regression modeling

---

## 2. 📐 Variance Analysis

A variance-based filtering step was applied to remove features with very low variability.

A variance threshold of **0.01** was used.

### Result

* **20 features were removed**
* The remaining features were carried forward to the next stages of analysis

This step helps eliminate features that contain very limited variation across observations and are therefore less likely to provide useful information for the model.

---

## 3. 🔗 Correlation Analysis

Correlation analysis was performed after the variance-based filtering step to investigate relationships between numerical variables.

The analysis was used to:

* Identify strongly related variables
* Detect potentially redundant information
* Better understand relationships within the feature space
* Support the subsequent feature-selection process

Correlation analysis was treated as an exploratory and diagnostic step rather than the only criterion for selecting predictive features.

---

## 4. 🎯 Feature Selection with Mutual Information

After the initial filtering and correlation analysis, **SelectKBest with Mutual Information (MI)** was used for feature selection.

Mutual Information measures the dependency between variables and can capture relationships that may not be purely linear.

### Feature Selection Process

```text
Filtered Feature Set
        │
        ▼
Mutual Information
        │
        ▼
SelectKBest
        │
        ▼
200 Selected Features
```

The SelectKBest step selected the **top 200 features** based on their Mutual Information scores.

These 200 features were then used as the main feature set for the Random Forest modeling stage.

---

## 5. 🌲 Random Forest Regression

A **Random Forest Regressor** was trained using the selected features.

Random Forest was used because it is well suited to nonlinear relationships and high-dimensional tabular datasets.

The model also provides feature-importance estimates, which were used in the next stage to investigate the relative contribution of individual variables.

---

## 6. 🧠 Random Forest Feature Importance

After training the Random Forest model, feature importance was analyzed to identify the features that contributed most to the model's predictions.

This provided a model-based perspective on feature relevance in addition to the statistical selection performed using Mutual Information.

The workflow therefore combines two different approaches:

```text
Statistical Feature Selection
Mutual Information
        │
        ▼
Top 200 Features
        │
        ▼
Random Forest
        │
        ▼
Model-Based Feature Importance
        │
        ▼
Feature Ranking
```

This ranking was then used to investigate whether smaller feature subsets could achieve comparable predictive performance.


---

# 📊 Model Evaluation

### Evaluation Metric Used in This Experiment

**Mean Absolute Error (MAE)**

MAE measures the average absolute difference between the predicted and actual target values.

Lower values indicate smaller average prediction errors.

### Feature Selection Comparison

```text
50 Features
MAE = 4,984,716.41

100 Features
MAE = 4,991,912.71

150 Features
MAE = 4,982,318.38

200 Features
MAE = 4,645,142.20
```

The comparison shows a noticeable improvement when moving from the smaller feature subsets to the full **200 selected features**.

---


