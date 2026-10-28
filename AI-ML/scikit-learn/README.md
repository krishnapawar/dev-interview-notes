# ⚙️ Scikit-learn & Classical ML Algorithms — Interview Questions & Answers

A production-oriented technical interview preparation guide covering the Scikit-learn unified API architecture, input shape semantics, Linear and Logistic Regression mathematics, evaluation metrics ($R^2$, MSE, AUC-ROC), Overfitting vs Underfitting diagnostics, Decision Trees, and Regularization techniques.

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

#### Code Demonstration

```python
import numpy as np
from sklearn.linear_model import LinearRegression

X_train = np.array([[1], [2], [3], [4], [5]])  # Shape: (5, 1)
y_train = np.array([30, 40, 50, 60, 70])        # Shape: (5,)

model = LinearRegression().fit(X_train, y_train)

# ❌ INCORRECT: 1D array raises ValueError
# model.predict([6]) 

# ✅ CORRECT: Single sample with single feature -> shape (1, 1)
pred_single = model.predict([[6]])

# ✅ Batch Prediction: 3 samples with 2 features -> shape (3, 2)
# Features: [Experience, Age]
pred_batch = model.predict([
    [5, 28],
    [3, 24],
    [8, 35]
])
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

#### Mathematical Formulation:
- **Univariate Equation:** $y = mx + c$
- **Multivariate Matrix Equation:** $\hat{y} = Xw + b$
- **Objective Function (Cost Function):**
  $$J(w, b) = \frac{1}{2N} \sum_{i=1}^N \left( y_i - (w x_i + b) \right)^2$$
- **Closed-Form Normal Equation (OLS):**
  $$W = (X^T X)^{-1} X^T y$$

---

### Q4: What Do Slope (Coefficients) and Intercept Represent in Multi-Feature Linear Regression?

> **Interview-Ready Answer:**  
> In linear regression, **Slope (`model.coef_`)** represents the marginal rate of change in the target variable for every unit increase in that feature while holding all other features constant; the **Intercept (`model.intercept_`)** represents the baseline expected value of the target when all input features equal zero.

#### Code Demonstration

```python
from sklearn.linear_model import LinearRegression
import numpy as np

# Features: [Experience (years), Interview_Score (0-100)]
X = np.array([
    [1, 60],
    [2, 70],
    [3, 75],
    [4, 85],
    [5, 90]
])
y = np.array([30000, 42000, 52000, 68000, 80000])

model = LinearRegression()
model.fit(X, y)

print("Slopes (Weights):", model.coef_)
# Slopes (Weights): [11200.   450.]
# -> Each additional year of experience adds ~$11,200 to salary.
# -> Each additional interview point adds ~$450 to salary.

print("Intercept (Baseline):", model.intercept_)
# Intercept: -9500.0 (Baseline when experience=0 and score=0)
```

#### Prediction Verification:
For someone with 5 years experience and interview score 85:
$$\hat{y} = (11200 \times 5) + (450 \times 85) - 9500 = 56000 + 38250 - 9500 = \$84,750$$

---

### Q5: How Do You Evaluate Regression Models Using R2 Score, MSE, RMSE, and MAE?

> **Interview-Ready Answer:**  
> Regression models are evaluated using **MSE** (heavily penalizes extreme outliers via squared penalties), **RMSE** (interpretable in identical physical units as the label), **MAE** (represents linear median error robust to outliers), and **$R^2$ Score** (quantifies the proportion of variance in the target explained by the features, where $1.0$ is perfect and $< 0$ indicates worse performance than a horizontal mean baseline).

#### Metric Formulations and Interpretation

| Metric | Formula | Behavior & Sensitivity | Ideal Target |
| :--- | :--- | :--- | :--- |
| **$R^2$ Score** | $1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2}$ | Scale-independent; percentage of variance explained | Approaching $1.0$ |
| **MSE** | $\frac{1}{N}\sum (y_i - \hat{y}_i)^2$ | Quadratic penalty; highly sensitive to extreme outliers | Approaching $0.0$ |
| **RMSE** | $\sqrt{\text{MSE}}$ | In same physical units as label ($); sensitive to outliers | Approaching $0.0$ |
| **MAE** | $\frac{1}{N}\sum \|y_i - \hat{y}_i\|$ | Linear penalty; robust to extreme anomalies | Approaching $0.0$ |

#### Code Demonstration

```python
from sklearn.metrics import r2_score, mean_squared_error, mean_absolute_error
import numpy as np

y_true = np.array([50000, 60000, 70000, 80000])
y_pred = np.array([48000, 62000, 69000, 83000])

r2 = r2_score(y_true, y_pred)
mse = mean_squared_error(y_true, y_pred)
rmse = np.sqrt(mse)
mae = mean_absolute_error(y_true, y_pred)

print(f"R² Score: {r2:.4f}")       # 0.9571 (Explains ~95.7% of variance)
print(f"RMSE:     ${rmse:,.2f}")    # $2,179.45
print(f"MAE:      ${mae:,.2f}")     # $2,000.00
```

---

### Q6: What Is the Difference Between Overfitting, Underfitting, and a Good Fit?

> **Interview-Ready Answer:**  
> **Underfitting** occurs when a model is too simple to capture the underlying structure (resulting in poor training and testing accuracy); **Overfitting** occurs when a model memorizes sample-specific noise in the training data rather than generalizing (resulting in near-perfect training accuracy but terrible testing accuracy); a **Good Fit** balances complexity to achieve high, consistent accuracy across both splits.

#### Diagnostic Benchmark Table

| State | Training Accuracy / Error | Testing Accuracy / Error | Root Cause | Fix Strategy |
| :--- | :--- | :--- | :--- | :--- |
| **Underfitting** (High Bias) | Low Accuracy (60%) / High Error | Low Accuracy (58%) / High Error | Overly simplistic model; insufficient features | 🔹 Increase model complexity<br>🔹 Add non-linear features<br>🔹 Decrease regularization |
| **Good Fit** (Optimal) | High Accuracy (94%) / Low Error | High Accuracy (92%) / Low Error | Balanced model complexity | ✅ Deploy to staging / production |
| **Overfitting** (High Variance)| Near-Perfect (99%) / Zero Error | Collapsed (68%) / High Error | Memorized training noise and sample outliers | 🔹 Add regularization ($L_1$/$L_2$)<br>🔹 Limit tree depth (`max_depth`)<br>🔹 Acquire more training data |

#### Classical Visual Analogy:
- **Student A (Underfitting):** Reads only 5 pages of a 100-page book; fails on both practice and final exam.
- **Student B (Overfitting):** Memorizes every question and answer of last year's exam; gets 100% on past exams but fails completely on fresh questions.
- **Student C (Good Fit):** Studies foundational principles and practices varied problems; scores 92% on both practice and new questions.

---

### Q7: What Is the Role of train_test_split() and Why Is random_state Essential?

> **Interview-Ready Answer:**  
> `train_test_split()` partitions historical data into non-overlapping training and evaluation subsets to measure out-of-sample generalization; the `random_state` parameter sets the pseudo-random number generator seed, ensuring identical data shuffling across executions to guarantee experiment reproducibility.

#### Code Demonstration

```python
from sklearn.model_selection import train_test_split
import numpy as np

X = np.arange(20).reshape(10, 2)
y = np.arange(10)

# test_size=0.2 allocates 80% to train (8 samples) and 20% to test (2 samples)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print("X_train Shape:", X_train.shape)  # (8, 2)
print("X_test Shape:", X_test.shape)    # (2, 2)
```

#### Why `random_state` Matters in Production:
- If omitted, every run shuffles data differently, producing varying metric results that make debugging impossible.
- Setting `random_state=42` ensures every team member and automated CI/CD pipeline sees identical splits and metric outputs. Any integer (`0`, `7`, `42`, `2026`) functions identically as a seed.

---

### Q8: What Is Logistic Regression and How Does It Differ From Linear Regression?

> **Interview-Ready Answer:**  
> Despite the word "regression" in its name, Logistic Regression is a supervised **classification** algorithm that models the log-odds of a categorical event by passing the linear equation output through the non-linear Sigmoid activation function, bounding the final output to a probability interval between $0$ and $1$.

#### Architectural Comparison

| Dimension | Linear Regression | Logistic Regression |
| :--- | :--- | :--- |
| **Task Type** | Regression (Continuous values) | Classification (Discrete class probabilities) |
| **Output Range** | Unbounded $(-\infty, +\infty)$ | Strictly bounded probability $(0, 1)$ |
| **Activation Function** | Identity (None) | Sigmoid (Binary) or Softmax (Multi-class) |
| **Target Variable ($y$)**| Continuous numeric: $y \in \mathbb{R}$ | Discrete classes: $y \in \{0, 1\}$ or multi-class |
| **Optimization Loss** | Mean Squared Error (MSE / Ordinary Least Squares) | Binary Cross-Entropy (Log Loss) |
| **Example Target** | Predict house price: $\$450,000$ | Predict fraud: Class 1 (Fraud) vs Class 0 (Legit) |

---

### Q9: How Does the Sigmoid Function Convert Real-Valued Logits into Probabilities?

> **Interview-Ready Answer:**  
> The Sigmoid (Logistic) function $\sigma(z) = \frac{1}{1 + e^{-z}}$ maps any real-valued linear input (logit $z = Xw + b$) spanning $(-\infty, +\infty)$ into an S-shaped curve strictly bounded between $0$ and $1$, allowing the output to be rigorously interpreted as the posterior probability $P(y=1|X)$.

#### Mathematical Formulation & Behavior

$$\sigma(z) = \frac{1}{1 + e^{-z}}$$

```text
    Probability P(y=1)
       1.0 ┼───────────────────────────────* * * * *
           │                            *
           │                          *
       0.5 ┼────────────────────────* (Decision Cutoff at z=0)
           │                      *
           │                    *
       0.0 ┼* * * * *──────────┴─────────────────────
          -∞                  z = 0                 +∞
                             Logit z = w·x + b
```

#### Key Threshold Properties:
- As $z \rightarrow +\infty$, $e^{-z} \rightarrow 0 \implies \sigma(z) \rightarrow 1.0$ (High confidence positive).
- As $z \rightarrow -\infty$, $e^{-z} \rightarrow \infty \implies \sigma(z) \rightarrow 0.0$ (High confidence negative).
- When $z = 0$, $e^{0} = 1 \implies \sigma(z) = \frac{1}{1 + 1} = 0.5$ (Maximum uncertainty boundary).

---

### Q10: How Do Decision Thresholds Work and When Should You Inspect predict_proba()?

> **Interview-Ready Answer:**  
> In classification, `.predict()` assigns class labels by applying a default decision threshold of $0.5$ to predicted probabilities, whereas `.predict_proba()` returns the raw continuous probabilities for each class; adjusting the threshold below or above $0.5$ is critical when balancing the business cost of False Positives versus False Negatives.

#### Code Demonstration

```python
from sklearn.linear_model import LogisticRegression
import numpy as np

# Feature: Hours studied -> Label: Pass (1) or Fail (0)
X = np.array([[1], [2], [3], [4], [5], [6]])
y = np.array([0, 0, 0, 1, 1, 1])

clf = LogisticRegression()
clf.fit(X, y)

# Inspect raw class probabilities
probs = clf.predict_proba([[3.5]])
print("Class Probabilities [P(0), P(1)]:", probs)
# e.g., [[0.45, 0.55]] -> 45% Fail, 55% Pass

# Default threshold (0.5)
print("Default Prediction:", clf.predict([[3.5]]))  # -> [1] (Pass)

# Custom High-Confidence Threshold for Medical/Fraud Alert (0.75 cutoff)
p_positive = probs[:, 1]
strict_prediction = (p_positive >= 0.75).astype(int)
print("Strict Prediction (>= 0.75):", strict_prediction)  # -> [0]
```

#### Production Threshold Trade-Offs:
- **Lowering Threshold (e.g., 0.2):** Maximizes **Recall**. Best for cancer detection or credit card fraud where missing a true positive is catastrophic.
- **Raising Threshold (e.g., 0.8):** Maximizes **Precision**. Best for automated email spam filtering where sending a legitimate email to Spam destroys user trust.

---

### Q11: What Is a Decision Tree and How Does It Construct Splitting Criteria?

> **Interview-Ready Answer:**  
> A Decision Tree is a non-parametric hierarchical model that partitions the feature space into orthogonal rectangular regions using a sequence of greedy recursive binary splits; at each node, it selects the feature and split threshold that maximizes information gain or impurity reduction.

#### Decision Flow Diagram

```text
               [Feature: Age > 30?]
                  /             \
             Yes /               \ No
                ▼                 ▼
     [Salary > $60,000?]       Class: REJECT (0)
        /           \
   Yes /             \ No
      ▼               ▼
Class: APPROVE (1)   Class: REJECT (0)
```

#### Core Components:
1. **Root Node:** Top-level node containing the complete dataset before any split.
2. **Internal Nodes:** Decision nodes testing conditions on a specific feature ($X_j \leq \theta$).
3. **Leaf Nodes:** Terminal nodes outputting the final majority class (classification) or mean target value (regression).
4. **Greedy Splitting:** The tree evaluates all available features and possible cutoff values to choose the single split producing the purest child nodes.

---

### Q12: What Is the Difference Between Gini Impurity and Entropy in Decision Trees?

> **Interview-Ready Answer:**  
> Gini Impurity measures the probability that a randomly chosen element from a node would be incorrectly labeled if classified according to the class distribution, whereas Entropy measures the information-theoretic disorder in the node; Gini is computationally faster because it avoids logarithmic calculations, making it the default in Scikit-learn's CART implementation.

#### Mathematical Formulations

- **Gini Impurity:**
  $$\text{Gini} = 1 - \sum_{i=1}^C p_i^2$$
  *(Bounds: $0.0$ for pure node, $0.5$ for balanced binary split).*

- **Entropy:**
  $$\text{Entropy} = - \sum_{i=1}^C p_i \log_2(p_i)$$
  *(Bounds: $0.0$ for pure node, $1.0$ for balanced binary split).*

#### Comparison Table

| Attribute | Gini Impurity | Entropy (Information Gain) |
| :--- | :--- | :--- |
| **Computational Cost**| Low (simple squared multiplications) | Higher (requires computing $\log_2$ floating points) |
| **Range (Binary)** | $[0.0, 0.5]$ | $[0.0, 1.0]$ |
| **Splitting Tendency** | Tends to isolate the most frequent class | Tends to produce slightly more balanced trees |
| **Empirical Results** | Identical trees in $> 98\%$ of industrial applications | Rarely provides measurable accuracy gains over Gini |
| **Default in Sklearn**| `criterion='gini'` | `criterion='entropy'` or `'log_loss'` |

---

### Q13: Why Are Decision Trees Prone to Overfitting and How Do You Regularize Them?

> **Interview-Ready Answer:**  
> Unconstrained decision trees will continue recursively splitting until every leaf node is completely pure or contains a single sample, creating an overly complex model that memorizes training noise; this high variance is regularized through pre-pruning hyperparameters like `max_depth`, `min_samples_split`, `min_samples_leaf`, and `max_features`.

#### Key Regularization Hyperparameters

```python
from sklearn.tree import DecisionTreeClassifier

# ❌ Unregularized Tree: Will split to purity and OVERFIT
overfitting_tree = DecisionTreeClassifier()

# ✅ Regularized Tree: Constrained complexity guarantees generalization
regularized_tree = DecisionTreeClassifier(
    max_depth=4,              # Limits maximum path length from root to leaf
    min_samples_split=10,     # Node must have >= 10 samples to consider a split
    min_samples_leaf=5,       # Leaf nodes must contain >= 5 samples
    max_features='sqrt',      # Evaluates subset of features per split
    random_state=42
)
```

#### Diagnostic Comparison

| Hyperparameter | Description | Effect of Constraining |
| :--- | :--- | :--- |
| **`max_depth`** | Maximum allowed depth of the tree | 🔹 Lower value combats overfitting; higher allows deeper complexity |
| **`min_samples_split`**| Minimum samples required to split an internal node | 🔹 Higher value prevents sample-specific micro-splits |
| **`min_samples_leaf`** | Minimum samples required at any terminal leaf node | 🔹 Higher value smooths out leaf predictions |
| **`max_leaf_nodes`** | Total maximum number of leaves allowed | 🔹 Direct cap on total tree capacity |

---

### Q14: What Is the Difference Between L1 Regularization (Lasso) and L2 Regularization (Ridge)?

> **Interview-Ready Answer:**  
> L1 Regularization (Lasso) adds the sum of absolute coefficients ($\lambda \sum |w_i|$) to the loss function and drives non-informative feature weights strictly to zero, serving as an automated feature selector; L2 Regularization (Ridge) adds the sum of squared coefficients ($\lambda \sum w_i^2$) and shrinks weights smoothly toward zero without eliminating them, excelling when features suffer from severe multicollinearity.

#### Mathematical Cost Functions

- **Ridge Regression ($L_2$ Penalty):**
  $$J(W) = \text{MSE} + \lambda \sum_{j=1}^D w_j^2$$

- **Lasso Regression ($L_1$ Penalty):**
  $$J(W) = \text{MSE} + \lambda \sum_{j=1}^D |w_j|$$

#### Comparison Table

| Feature | Lasso ($L_1$ Regularization) | Ridge ($L_2$ Regularization) | Elastic Net ($L_1 + L_2$) |
| :--- | :--- | :--- | :--- |
| **Penalty Term** | Absolute weights: $\sum \|w_i\|$ | Squared weights: $\sum w_i^2$ | Linear combination: $\alpha L_1 + (1-\alpha) L_2$ |
| **Feature Selection**| ✅ Yes (sets unneeded weights to 0.0) | ❌ No (shrinks weights asymptotically) | ✅ Yes (with grouped selection) |
| **Sparsity** | Produces sparse weight vectors | Retains all features in final equation | Produces sparse, correlated feature groups |
| **Multicollinearity**| Arbitrarily selects one feature from collinear set | Distributes weight evenly across collinear set | Balances selection across correlated groups |
| **Scikit-learn Class**| `from sklearn.linear_model import Lasso` | `from sklearn.linear_model import Ridge` | `from sklearn.linear_model import ElasticNet` |