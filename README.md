# Portfolio-project-on-optimising-a-model-for-real-life-data

## LightGBM – Bayesian Optimized Version

### Model Overview
This LightGBM classifier was trained to predict ad click behavior using a dataset containing features like user demographics (age, gender), device usage patterns, and ad positions. To improve predictive performance and handle class imbalance, Bayesian Optimization via Optuna was used to tune key hyperparameters.

### Objective
Predict whether a user will click on an ad (binary classification).

### Input Features
- `age`
- `gender`
- `device_type`
- `ad_position`
- `time_of_day`
- `age_bin` (binned age)
- `device_hour` (composite feature)

### Handling Imbalance
This model uses a combination of:
- **Class weighting** (`scale_pos_weight`)
- **Bayesian Optimization** to tune this weight along with other hyperparameters

---

### Hyperparameters (Tuned via Optuna)
| Parameter            | Value        |
|----------------------|--------------|
| `learning_rate`      | 0.16199      |
| `num_leaves`         | 148          |
| `max_depth`          | 13           |
| `min_child_samples`  | 10           |
| `feature_fraction`   | 0.89895      |
| `bagging_fraction`   | 0.68144      |
| `scale_pos_weight`   | 2.53918      |
| `num_boost_round`    | 200          |

---

### Performance (Test Set)

| Metric               | Value        |
|----------------------|--------------|
| AUC Score            | 0.6777       |
| Accuracy             | 0.70         |
| Precision (click=1)  | 0.70         |
| Recall (click=1)     | 0.94         |
| F1-score (click=1)   | 0.80         |
| Confusion Matrix     | `[[186, 519], [ 76, 1219]]` |

---

### Comparison with Class Weighting (No Optimization)
| Metric               | Class Weighting | Bayesian Optimized |
|----------------------|------------------|---------------------|
| AUC Score            | 0.6519           | **0.6777** ✅        |
| Accuracy             | 0.69             | **0.70** ✅          |
| F1-score (click=1)   | 0.80             | **0.80** (same)     |
| Recall (click=1)     | 0.98             | 0.94                |
| Precision (click=1)  | 0.68             | **0.70** ✅          |

---

### Recommended Threshold (Optimized via F1)
- **Best Threshold**: `0.48`
- **Best F1 Score**: `0.8031`

---

### Intended Use
- Improve CTR prediction in online advertising
- Target users with more relevant and timely ads
- Allocate ad budget more effectively

---

### Limitations
- Model performance may degrade on data with significantly different distributions.
- Precision-recall tradeoff must be re-tuned if business requirements change (e.g., false positive tolerance).
- Assumes all categorical values seen in training will exist during inference.

