# ⚙️ Scikit-learn & Classical ML Algorithms — Master Hub

A production-oriented technical interview preparation guide covering the Scikit-learn unified API architecture, regression, classification, ensemble trees, evaluation metrics, and distance/margin-based learning algorithms.

---

## 📂 Module Breakdown

This comprehensive guide is partitioned into three dedicated, in-depth technical interview guides:

| Guide | Core Focus Areas | Direct Link |
| :--- | :--- | :--- |
| **📈 Part 1: Regression & Validation** | Linear Regression, OLS minimization, $y=mx+c$, $R^2$, MSE, RMSE, MAE, Overfitting vs Underfitting, Bias-Variance, `train_test_split()`, $L_1$/$L_2$ Regularization | [**Open Part 1 Guide**](./01-regression-and-validation.md) |
| **🌲 Part 2: Classification & Trees** | Logistic Regression, Sigmoid function, decision thresholds, `predict_proba()`, Decision Trees, Gini Impurity vs Entropy, pre-pruning (`max_depth`), Random Forest bagging & majority voting | [**Open Part 2 Guide**](./02-classification-and-trees.md) |
| **🎯 Part 3: Metrics, KNN & SVM** | Confusion Matrix (TP, TN, FP, FN), Precision vs Recall trade-offs, F1-Score, K-Nearest Neighbors (KNN), Euclidean distance, feature scaling, Support Vector Machines (SVM) & Kernel Trick | [**Open Part 3 Guide**](./03-model-evaluation-knn-svm.md) |

---

## 📋 Table of Contents

1. [What Is Scikit-learn and How Does Its Unified API Architecture Function?](#q1-what-is-scikit-learn-and-how-does-its-unified-api-architecture-function)
2. [Why Does Scikit-learn Strictly Require a 2D Array (n_samples, n_features) for Feature Inputs?](#q2-why-does-scikit-learn-strictly-require-a-2d-array-n_samples-n_features-for-feature-inputs)
3. [How Does Linear Regression Work and How Is the Best-Fit Line Equation Calculated?](#q3-how-does-linear-regression-work-and-how-is-the-best-fit-line-equation-calculated)
4. [What Do Slope (Coefficients) and Intercept Represent in Multi-Feature Linear Regression?](#q4-what-do-slope-coefficients-and-intercept-represent-in-multi-feature-linear-regression)
5. [How Do You Evaluate Regression Models Using R2 Score, MSE, RMSE, and MAE?](#q5-how-do-you-evaluate-regression-models-using-r2-score-mse-rmse-and-mae)
6. [What Is the Difference Between Overfitting, Underfitting, and a Good Fit?](#q6-what-is-the-difference-between-overfitting-underfitting-and-a-good-fit)
7. [What Is the Role of train_test_split() and Why Is random_state Essential?](#q7-what-is-the-role-of-train_test_split-and-why-is-random_state-essential)
8. [What Is Logistic Regression and How Does It Differ From Linear Regression?](#q8-what-is-logistic-regression-and-how-does-it-differ-from-linear-regression)
9. [How Does the Sigmoid Function Convert Real-Valued Logits into Probabilities?](#q9-how-does-the-sigmoid-function-convert-real-valued-logits-into-probabilities)
10. [How Do Decision Thresholds Work and When Should You Inspect predict_proba()?](#q10-how-do-decision-thresholds-work-and-when-should-you-inspect-predict_proba)
11. [What Is a Decision Tree and How Does It Construct Splitting Criteria?](#q11-what-is-a-decision-tree-and-how-does-it-construct-splitting-criteria)
12. [What Is the Difference Between Gini Impurity and Entropy in Decision Trees?](#q12-what-is-the-difference-between-gini-impurity-and-entropy-in-decision-trees)
13. [Why Are Decision Trees Prone to Overfitting and How Do You Regularize Them?](#q13-why-are-decision-trees-prone-to-overfitting-and-how-do-you-regularize-them)
14. [What Is the Difference Between L1 Regularization (Lasso) and L2 Regularization (Ridge)?](#q14-what-is-the-difference-between-l1-regularization-lasso-and-l2-regularization-ridge)

---

### Q1: What Is Scikit-learn and How Does Its Unified API Architecture Function?

> **Interview-Ready Answer:**  
> Scikit-learn is Python's gold-standard machine learning library built on NumPy, SciPy, and Matplotlib; it organizes all algorithms through a clean object-oriented interface centered around three core method patterns: **Estimators** (`.fit()`), **Transformers** (`.transform()`, `.fit_transform()`), and **Predictors** (`.predict()`, `.predict_proba()`).

#### The Unified Interface Pattern

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ 1. Estimator: Computes model parameters from data                           │
│    model.fit(X_train, y_train)                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│ 2. Transformer: Modifies feature representations (scalers, encoders)        │
│    X_scaled = scaler.transform(X_test)                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│ 3. Predictor: Produces predictions or class probabilities on unseen inputs │
│    y_pred = model.predict(X_test)                                           │
│    y_prob = model.predict_proba(X_test)                                     │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Code Demonstration

```python
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import StandardScaler
import numpy as np

X = np.array([[1.0], [2.0], [3.0], [4.0]])
y = np.array([30000, 45000, 60000, 75000])

# 1. Transformer interface
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# 2. Estimator and Predictor interface
regressor = LinearRegression()
regressor.fit(X_scaled, y)
predictions = regressor.predict(scaler.transform([[5.0]]))
print(f"Predicted Output: ${predictions[0]:,.2f}")
```

---

### Q2: Why Does Scikit-learn Strictly Require a 2D Array (n_samples, n_features) for Feature Inputs?

> **Interview-Ready Answer:**  
> Scikit-learn enforces a strict 2D shape convention `(n_samples, n_features)` for feature matrix $X$ because matrix algebra operations require explicit row and column dimensions; passing a 1D vector of shape `(n_samples,)` creates ambiguity because the algorithm cannot distinguish between an array of multiple samples with one feature versus one sample with multiple features.

#### Shape Mechanics: Why `[[6]]` Is Required Instead of `[6]`

```text
Scalar in 1D array: [6]
Shape: (1,)  ──► ❌ Ambiguous: Is this 1 sample with 1 feature, or 6 samples?
                 Scikit-learn raises ValueError: Expected 2D array, got 1D array.

Scalar in 2D array: [[6]]
Shape: (1, 1) ──► ✅ Explicit: Exactly 1 sample (row) with 1 feature (column).
```

---

### Q3: How Does Linear Regression Work and How Is the Best-Fit Line Equation Calculated?

> **Interview-Ready Answer:**  
> Linear Regression models the relationship between dependent continuous variable $y$ and independent features $X$ using the linear equation $y = XW + b$; it determines the optimal parameters ($W, b$) by minimizing the Sum of Squared Residuals (SSR) between actual observations and predictions using Ordinary Least Squares (OLS) or Gradient Descent.

#### The Best-Fit Line Mechanics

```text
   y (Salary)
       │
80k ───┼───────────────────────────* (Actual)
       │                         / │
60k ───┼───────────* (Actual)   /  │ Residual Error: e = y - ŷ
       │          / │          /   * (Predicted: ŷ = mx + c)
40k ───┼───*     /  * (ŷ)     /
       │  / │   /            /
20k ───* (c: Intercept)     /  Slope m = Δy / Δx
       │                   /
       └───────────────────────────────► x (Experience)
```

---

### Q4: What Do Slope (Coefficients) and Intercept Represent in Multi-Feature Linear Regression?

> **Interview-Ready Answer:**  
> In linear regression, **Slope (`model.coef_`)** represents the marginal rate of change in the target variable for every unit increase in that feature while holding all other features constant; the **Intercept (`model.intercept_`)** represents the baseline expected value of the target when all input features equal zero.

---

### Q5: How Do You Evaluate Regression Models Using R2 Score, MSE, RMSE, and MAE?

> **Interview-Ready Answer:**  
> Regression models are evaluated using **MSE** (heavily penalizes extreme outliers via squared penalties), **RMSE** (interpretable in identical physical units as the label), **MAE** (represents linear median error robust to outliers), and **$R^2$ Score** (quantifies the proportion of variance in the target explained by the features, where $1.0$ is perfect and $< 0$ indicates worse performance than a horizontal mean baseline).

| Metric | Formula | Behavior & Sensitivity | Ideal Target |
| :--- | :--- | :--- | :--- |
| **$R^2$ Score** | $1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2}$ | Scale-independent; percentage of variance explained | Approaching $1.0$ |
| **MSE** | $\frac{1}{N}\sum (y_i - \hat{y}_i)^2$ | Quadratic penalty; highly sensitive to extreme outliers | Approaching $0.0$ |
| **RMSE** | $\sqrt{\text{MSE}}$ | In same physical units as label ($); sensitive to outliers | Approaching $0.0$ |
| **MAE** | $\frac{1}{N}\sum \|y_i - \hat{y}_i\|$ | Linear penalty; robust to extreme anomalies | Approaching $0.0$ |

---

### Q6: What Is the Difference Between Overfitting, Underfitting, and a Good Fit?

> **Interview-Ready Answer:**  
> **Underfitting** occurs when a model is too simple to capture the underlying structure (resulting in poor training and testing accuracy); **Overfitting** occurs when a model memorizes sample-specific noise in the training data rather than generalizing (resulting in near-perfect training accuracy but terrible testing accuracy); a **Good Fit** balances complexity to achieve high, consistent accuracy across both splits.

---

### Q7: What Is the Role of train_test_split() and Why Is random_state Essential?

> **Interview-Ready Answer:**  
> `train_test_split()` partitions historical data into non-overlapping training and evaluation subsets to measure out-of-sample generalization; the `random_state` parameter sets the pseudo-random number generator seed, ensuring identical data shuffling across executions to guarantee experiment reproducibility.

---

### Q8: What Is Logistic Regression and How Does It Differ From Linear Regression?

> **Interview-Ready Answer:**  
> Despite the word "regression" in its name, Logistic Regression is a supervised **classification** algorithm that models the log-odds of a categorical event by passing the linear equation output through the non-linear Sigmoid activation function, bounding the final output to a probability interval between $0$ and $1$.

---

### Q9: How Does the Sigmoid Function Convert Real-Valued Logits into Probabilities?

> **Interview-Ready Answer:**  
> The Sigmoid (Logistic) function $\sigma(z) = \frac{1}{1 + e^{-z}}$ maps any real-valued linear input (logit $z = Xw + b$) spanning $(-\infty, +\infty)$ into an S-shaped curve strictly bounded between $0$ and $1$, allowing the output to be rigorously interpreted as the posterior probability $P(y=1|X)$.

---

### Q10: How Do Decision Thresholds Work and When Should You Inspect predict_proba()?

> **Interview-Ready Answer:**  
> In classification, `.predict()` assigns class labels by applying a default decision threshold of $0.5$ to predicted probabilities, whereas `.predict_proba()` returns the raw continuous probabilities for each class; adjusting the threshold below or above $0.5$ is critical when balancing the business cost of False Positives versus False Negatives.

---

### Q11: What Is a Decision Tree and How Does It Construct Splitting Criteria?

> **Interview-Ready Answer:**  
> A Decision Tree is a non-parametric hierarchical model that partitions the feature space into orthogonal rectangular regions using a sequence of greedy recursive binary splits; at each node, it selects the feature and split threshold that maximizes information gain or impurity reduction.

---

### Q12: What Is the Difference Between Gini Impurity and Entropy in Decision Trees?

> **Interview-Ready Answer:**  
> Gini Impurity measures the probability that a randomly chosen element from a node would be incorrectly labeled if classified according to the class distribution, whereas Entropy measures the information-theoretic disorder in the node; Gini is computationally faster because it avoids logarithmic calculations, making it the default in Scikit-learn's CART implementation.

---

### Q13: Why Are Decision Trees Prone to Overfitting and How Do You Regularize Them?

> **Interview-Ready Answer:**  
> Unconstrained decision trees will continue recursively splitting until every leaf node is completely pure or contains a single sample, creating an overly complex model that memorizes training noise; this high variance is regularized through pre-pruning hyperparameters like `max_depth`, `min_samples_split`, `min_samples_leaf`, and `max_features`.

---

### Q14: What Is the Difference Between L1 Regularization (Lasso) and L2 Regularization (Ridge)?

> **Interview-Ready Answer:**  
> L1 Regularization (Lasso) adds the sum of absolute coefficients ($\lambda \sum |w_i|$) to the loss function and drives non-informative feature weights strictly to zero, serving as an automated feature selector; L2 Regularization (Ridge) adds the sum of squared coefficients ($\lambda \sum w_i^2$) and shrinks weights smoothly toward zero without eliminating them, excelling when features suffer from severe multicollinearity.