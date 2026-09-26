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

# ⚖️ Feature Subset Comparison

To evaluate the effect of dimensionality reduction, Random Forest models were compared using different numbers of features.

The following feature subsets were evaluated:

* 50 features
* 100 features
* 150 features
* 200 features

The models were evaluated using **Mean Absolute Error (MAE)**.

### Results

| Number of Features |              MAE |
| -----------------: | ---------------: |
|                 50 |     4,984,716.41 |
|                100 |     4,991,912.71 |
|                150 |     4,982,318.38 |
|                200 | **4,645,142.20** |

### Comparison

The results show that the **200-feature model achieved the lowest MAE among the tested feature subsets**.

Reducing the feature set to 50, 100, or 150 features resulted in higher MAE compared with the 200-feature configuration.

This indicates that, for the current Random Forest setup, retaining the larger selected feature set preserved additional predictive information that was lost when using smaller subsets.

> **Important:** These results are based on MAE. The original Kaggle competition metric is RMSLE, so RMSLE should be reported separately if it is calculated for the final models.

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

# 📈 Project Progress

| Stage                                  | Result / Status       |
| -------------------------------------- | --------------------- |
| Data Understanding                     | ✅ Completed           |
| Variance Analysis                      | ✅ Completed           |
| Variance Threshold                     | **0.01**              |
| Features Removed by Variance Filtering | **20**                |
| Correlation Analysis                   | ✅ Completed           |
| Mutual Information                     | ✅ Completed           |
| SelectKBest                            | ✅ Completed           |
| Selected Features                      | **200**               |
| Random Forest Regression               | ✅ Completed           |
| Random Forest Feature Importance       | ✅ Completed           |
| Feature Subset Comparison              | ✅ Completed           |
| 50-Feature Model                       | MAE: **4,984,716.41** |
| 100-Feature Model                      | MAE: **4,991,912.71** |
| 150-Feature Model                      | MAE: **4,982,318.38** |
| 200-Feature Model                      | **MAE: 4,645,142.20** |
| Final Model Optimization               | 🔄 Next Step          |

---

# 💡 Key Findings

Several observations emerged from the feature-selection and modeling experiments:

### 1. Low-variance filtering reduced the feature space

Using a variance threshold of **0.01 removed 20 features**, providing an initial reduction before more advanced feature-selection techniques.

### 2. Mutual Information identified 200 informative features

SelectKBest with Mutual Information was used to select the **200 highest-scoring features**, creating the main feature set for subsequent Random Forest modeling.

### 3. Smaller feature subsets did not outperform the 200-feature configuration

The comparison of 50, 100, 150, and 200 features showed that the **200-feature configuration produced the lowest MAE** among the tested alternatives.

### 4. Feature reduction has a trade-off

While reducing the number of features can simplify a model and reduce computational requirements, the experiments indicate that more aggressive feature reduction can also remove predictive information.

For this dataset and the current Random Forest configuration, the 200-feature representation retained more useful information than the smaller tested subsets.

---

# 🚀 Next Steps

The next stage of the project can focus on improving the current Random Forest model through:

* Hyperparameter tuning
* Cross-validation
* Testing additional tree-based models
* Comparing Random Forest with boosting algorithms
* Evaluating the models using RMSLE
* Further analysis of Random Forest feature importance
* Error analysis
* Final model optimization
