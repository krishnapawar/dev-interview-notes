# 🔢 NumPy — Interview Questions & Answers

A production-grade technical interview guide covering NumPy array memory architecture, multi-dimensional array manipulation, broadcasting rules, matrix mathematics, indexing and slicing semantics, and high-performance vectorized computation for machine learning.

---

## 📋 Table of Contents

1. [What Is NumPy and Why Is It Preferred Over Native Python Lists?](#q1-what-is-numpy-and-why-is-it-preferred-over-native-python-lists)
2. [How Do You Create 1D, 2D, and 3D Arrays and Inspect Shape, Dimension, and Data Types?](#q2-how-do-you-create-1d-2d-and-3d-arrays-and-inspect-shape-dimension-and-data-types)
3. [How Do You Initialize Special Arrays (zeros, ones, eye, arange) in NumPy?](#q3-how-do-you-initialize-special-arrays-zeros-ones-eye-arange-in-numpy)
4. [What Is the Difference Between Indexing arr[row, col] vs arr[row][col] in NumPy?](#q4-what-is-the-difference-between-indexing-arrrow-col-vs-arrrowcol-in-numpy)
5. [How Does Array Slicing Work in NumPy and What Is the Difference Between a View and a Copy?](#q5-how-does-array-slicing-work-in-numpy-and-what-is-the-difference-between-a-view-and-a-copy)
6. [How Does reshape() Work and What Are the Constraints on Array Dimensions?](#q6-how-does-reshape-work-and-what-are-the-constraints-on-array-dimensions)
7. [What Is the Difference Between flatten() and ravel() in NumPy?](#q7-what-is-the-difference-between-flatten-and-ravel-in-numpy)
8. [What Is the Difference Between Element-Wise Multiplication and Matrix Multiplication?](#q8-what-is-the-difference-between-element-wise-multiplication-and-matrix-multiplication)
9. [What Are NumPy Aggregate Functions and How Does the axis Parameter Function?](#q9-what-are-numpy-aggregate-functions-and-how-does-the-axis-parameter-function)
10. [What Is Array Broadcasting and What Rules Govern Dimension Compatibility?](#q10-what-is-array-broadcasting-and-what-rules-govern-dimension-compatibility)

---

### Q1: What Is NumPy and Why Is It Preferred Over Native Python Lists?

> **Interview-Ready Answer:**  
> NumPy is the fundamental C-based package for scientific computing in Python, providing the homogeneous `ndarray` data structure stored in contiguous memory blocks; it is orders of magnitude faster than standard Python lists because it avoids pointer indirection, eliminates dynamic type checking overhead, and executes operations via SIMD CPU vectorization.

#### Architectural Comparison: Python List vs NumPy `ndarray`

```text
Python List (Array of Pointers):
[ Pointer 0 ] ──► PyObject (Integer 1: type info, refcount, 28 bytes)
[ Pointer 1 ] ──► PyObject (Integer 2: type info, refcount, 28 bytes)
[ Pointer 2 ] ──► PyObject (Integer 3: type info, refcount, 28 bytes)
* Non-contiguous in RAM, poor CPU L1/L2 cache locality, 8x-10x memory bloat.

NumPy ndarray (Dense Contiguous Buffer):
┌───────────┬───────────┬───────────┐
│ Int64 (1) │ Int64 (2) │ Int64 (3) │  (Exact 8 bytes per item, zero pointer overhead)
└───────────┴───────────┴───────────┘
* Contiguous in RAM, optimal CPU prefetching, SIMD hardware acceleration.
```

#### Detailed Comparison Table

| Dimension | Native Python List | NumPy `ndarray` |
| :--- | :--- | :--- |
| **Memory Allocation** | Array of pointers pointing to scattered heap objects | Contiguous memory buffer of uniform primitive bytes |
| **Type Homogeneity** | Heterogeneous (can store strings, ints, objects together) | Strictly homogeneous (all elements share identical `dtype`) |
| **Execution Speed** | Interpreted Python loop with boxing/unboxing overhead | Compiled C/Fortran routines with SIMD vectorization |
| **Memory Footprint** | ~28 bytes per integer + 8 bytes pointer = ~36 bytes/item | Exactly 4 or 8 bytes per element (e.g., `int32`, `float64`) |
| **Mathematical Operations**| Concatenates on `+` (`[1] + [2] = [1, 2]`) | Performs element-wise vector math (`arr1 + arr2`) |

---

### Q2: How Do You Create 1D, 2D, and 3D Arrays and Inspect Shape, Dimension, and Data Types?

> **Interview-Ready Answer:**  
> NumPy arrays are created using `np.array()` from nested sequences; array geometry and memory structure are inspected using `.shape` (tuple of lengths per axis), `.ndim` (integer count of dimensions), `.dtype` (underlying element byte format), and `.size` (total element count).

#### Code Example: 1D, 2D, and 3D Construction

```python
import numpy as np

# 1D Array (Vector)
a = np.array([1, 2, 3, 4, 5], dtype=np.int64)

# 2D Array (Matrix: 2 rows, 3 columns)
b = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

# 3D Array (Tensor: 2 matrices of 2x2 shape)
c = np.array([
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]]
])

print("1D -> Shape:", a.shape, "| Ndim:", a.ndim, "| Dtype:", a.dtype, "| Size:", a.size)
# 1D -> Shape: (5,) | Ndim: 1 | Dtype: int64 | Size: 5

print("2D -> Shape:", b.shape, "| Ndim:", b.ndim, "| Dtype:", b.dtype, "| Size:", b.size)
# 2D -> Shape: (2, 3) | Ndim: 2 | Dtype: int64 | Size: 6

print("3D -> Shape:", c.shape, "| Ndim:", c.ndim, "| Dtype:", c.dtype, "| Size:", c.size)
# 3D -> Shape: (2, 2, 2) | Ndim: 3 | Dtype: int64 | Size: 8
```

#### Structural Summary
- **`shape`:** Indicates cardinality per axis. For a matrix of $(M, N)$, $M$ is row count, $N$ is column count.
- **`ndim`:** The number of axes (e.g., vector $= 1$, matrix $= 2$, image tensor $= 3$).
- **`dtype`:** Memory representation such as `int32`, `float32`, `float64`, or `bool`.

---

### Q3: How Do You Initialize Special Arrays (zeros, ones, eye, arange) in NumPy?

> **Interview-Ready Answer:**  
> Special arrays are initialized with dedicated factory functions without requiring Python list allocation: `np.zeros()` generates zero-filled arrays, `np.ones()` generates one-filled arrays, `np.eye()` creates an identity matrix with ones on the main diagonal, and `np.arange()` creates evenly spaced numerical sequences within a half-open interval.

#### Core Initialization Methods

```python
import numpy as np

# 1. Zeros Matrix (3 rows, 3 columns of 0.0)
zeros_mat = np.zeros((3, 3), dtype=np.float32)

# 2. Ones Matrix (2 rows, 4 columns of 1.0)
ones_mat = np.ones((2, 4), dtype=np.float64)

# 3. Identity Matrix (4x4 diagonal with 1.0, off-diagonals 0.0)
identity_mat = np.eye(4)

# 4. Numerical Range (start, stop, step) -> [0, 2, 4, 6, 8, 10]
evens_range = np.arange(0, 12, 2)

# 5. Linearly Spaced Array (start, stop, num_points)
lin_points = np.linspace(0.0, 1.0, num=5)  # [0.0, 0.25, 0.5, 0.75, 1.0]

print("Zeros:\n", zeros_mat)
print("Identity:\n", identity_mat)
print("Arange:\n", evens_range)
```

#### Production Context
- `np.zeros()` is commonly used to pre-allocate weight gradients or output image buffers.
- `np.eye()` is essential for covariance matrix initialization and regularization terms ($\lambda I$ in Ridge Regression).

---

### Q4: What Is the Difference Between Indexing arr[row, col] vs arr[row][col] in NumPy?

> **Interview-Ready Answer:**  
> While both return the target element, `arr[row, col]` is the native NumPy multi-dimensional indexing syntax that resolves the element in a single optimized C pointer offset calculation, whereas `arr[row][col]` executes two separate indexing steps by creating an intermediate row array object before indexing the second dimension, causing unnecessary performance overhead.

#### Execution Mechanics

```python
import numpy as np

arr = np.array([
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
])

# ✅ Preferred: Single multi-axis C-level lookup (fastest)
val_fast = arr[1, 2]  # Returns 60

# ⚠️ Discouraged: Creates intermediate 1D slice arr[1], then indexes [2]
val_slow = arr[1][2]  # Returns 60
```

#### Why Multi-Axis Indexing (`arr[r, c]`) Is Essential:
1. **Performance:** Single memory offset calculation: $\text{offset} = r \times \text{stride}_0 + c \times \text{stride}_1$.
2. **Advanced Multi-Dimensional Slicing:** Syntax like `arr[0:2, 1:3]` cannot be replicated with chained brackets (`arr[0:2][1:3]` slices the rows of the slice, not the columns).

---

### Q5: How Does Array Slicing Work in NumPy and What Is the Difference Between a View and a Copy?

> **Interview-Ready Answer:**  
> NumPy basic slicing (`arr[start:stop:step]`) creates a **view** rather than a copy, meaning it shares the exact same underlying memory buffer with the original array; mutating a slice directly mutates the parent array. To duplicate the data independently, an explicit `.copy()` call must be executed.

#### View vs Copy Code Demonstration

```python
import numpy as np

# 2D Array Slicing: arr[row_slice, col_slice]
mat = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

# Extract top-left 2x2 submatrix as a VIEW
sub_view = mat[0:2, 0:2]

# Mutating the view modifies original array!
sub_view[0, 0] = 999
print("Original Matrix Modified:\n", mat)
# mat[0, 0] is now 999!

# Explicit Copy: Independent memory allocation
safe_copy = mat[0:2, 0:2].copy()
safe_copy[0, 0] = 42
print("Original Matrix Remains Intact:\n", mat[0, 0])  # Still 999

# Slicing with negative step to reverse rows
reversed_rows = mat[::-1, :]
```

#### Comparison Table

| Attribute | NumPy Array View | NumPy Array Copy |
| :--- | :--- | :--- |
| **Memory Allocation** | Shares the existing memory buffer (`base` points to parent) | Allocates an entirely new contiguous memory block |
| **Mutation Effect** | Changes in view reflect in parent and vice-versa | Changes are isolated to the copied instance |
| **Check Mechanism** | `sub_arr.base is parent_arr` evaluates to `True` | `sub_arr.base is None` evaluates to `True` |
| **Computational Cost** | $\mathcal{O}(1)$ time and $\mathcal{O}(1)$ space | $\mathcal{O}(N)$ time and $\mathcal{O}(N)$ space |

---

### Q6: How Does reshape() Work and What Are the Constraints on Array Dimensions?

> **Interview-Ready Answer:**  
> `reshape()` reconfigures the dimensions of an array without altering its underlying memory data; it requires that the product of the new dimension lengths equals the exact total element count (`size`) of the original array, and allows one dimension to be inferred automatically using `-1`.

#### Code Example and Dimension Invariants

```python
import numpy as np

# 12 elements total
x = np.arange(1, 13)

# Valid Reshapes (Product must equal 12)
mat_3x4 = x.reshape(3, 4)   # 3 * 4 = 12
mat_2x6 = x.reshape(2, 6)   # 2 * 6 = 12
mat_4x3 = x.reshape(4, 3)   # 4 * 3 = 12

# Using -1 for Automatic Dimension Inference
# NumPy automatically calculates 12 / 4 = 3 rows
auto_inferred = x.reshape(-1, 4)  # Shape (3, 4)

print("Inferred Shape:", auto_inferred.shape)

# Converting to 2D column vector (critical for ML features)
col_vector = x.reshape(-1, 1)  # Shape (12, 1)
print("Column Vector Shape:", col_vector.shape)
```

#### Key Rules:
- `x.reshape(5, 3)` throws `ValueError: cannot reshape array of size 12 into shape (5,3)` because $5 \times 3 = 15 \neq 12$.
- The `-1` placeholder can only be used for **at most one** dimension per reshape operation.

---

### Q7: What Is the Difference Between flatten() and ravel() in NumPy?

> **Interview-Ready Answer:**  
> `flatten()` always returns an independent, deep copy of the array collapsed into a 1D sequence, allocating new memory; `ravel()` returns a contiguous flattened 1D view of the original array whenever possible without memory allocation, copying only if the original array is non-contiguous in memory.

#### Behavioral Comparison

```python
import numpy as np

arr = np.array([[1, 2], [3, 4]])

# 1. ravel(): Returns a VIEW whenever possible
r = arr.ravel()
r[0] = 100
print("Original after ravel mutation:\n", arr)  # arr[0,0] is now 100!

# 2. flatten(): ALWAYS returns a COPY
f = arr.flatten()
f[0] = 999
print("Original after flatten mutation:\n", arr)  # arr[0,0] is STILL 100!
```

#### Comparison Table

| Feature | `ravel()` | `flatten()` |
| :--- | :--- | :--- |
| **Return Type** | View (shared memory) if contiguous; copy otherwise | Always a new copy (independent memory) |
| **Memory Overhead** | Minimal $\mathcal{O}(1)$ | $\mathcal{O}(N)$ memory allocation |
| **Performance** | Much faster (no memory copy needed) | Slower for large arrays due to buffer allocation |
| **Use Case** | Quick flat iteration or passing to ML models | When you must guarantee safe, isolated mutations |

---

### Q8: What Is the Difference Between Element-Wise Multiplication and Matrix Multiplication?

> **Interview-Ready Answer:**  
> The `*` operator performs element-wise (Hadamard) multiplication requiring matching or broadcastable shapes, whereas `@` (or `np.matmul()`) computes the true algebraic matrix dot product where the inner dimensions must match: $(M \times K) \cdot (K \times N) \rightarrow (M \times N)$.

#### Mathematical Formulation

- **Element-Wise (`*`):** $C_{i,j} = A_{i,j} \times B_{i,j}$
- **Matrix Multiplication (`@`):** $C_{i,j} = \sum_{k=1}^K A_{i,k} \cdot B_{k,j}$

#### Code Example

```python
import numpy as np

a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

# 1. Element-Wise Multiplication
element_wise = a * b
# [[1*5, 2*6], [3*7, 4*8]] -> [[5, 12], [21, 32]]

# 2. Matrix Multiplication (Dot Product)
matrix_product = a @ b  # Identical to np.matmul(a, b)
# [[1*5 + 2*7, 1*6 + 2*8], [3*5 + 4*7, 3*6 + 4*8]]
# -> [[19, 22], [43, 50]]

print("Element-Wise:\n", element_wise)
print("Matrix Multiplication:\n", matrix_product)
```

#### Production ML Note
In neural networks and linear models, weight transformations $\hat{y} = XW + b$ strictly utilize matrix multiplication (`@`), not element-wise multiplication.

---

### Q9: What Are NumPy Aggregate Functions and How Does the axis Parameter Function?

> **Interview-Ready Answer:**  
> NumPy aggregate functions compute summary statistics across elements; the `axis` parameter defines the direction of dimension reduction: `axis=None` aggregates over the entire array to produce a scalar, `axis=0` aggregates vertically down columns (collapsing rows), and `axis=1` aggregates horizontally across rows (collapsing columns).

#### Axis Visualization

```text
         axis=1 (across columns -> collapses to row summaries)
             ──────►
       ┌───┬───┬───┐
axis=0 │ 1 │ 2 │ 3 │
  │    ├───┼───┼───┤
  ▼    │ 4 │ 5 │ 6 │
       └───┴───┴───┘
(down rows -> collapses to column summaries)
```

#### Code Demonstration

```python
import numpy as np

mat = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

# Global Aggregations (axis=None -> scalar)
print("Global Sum:", mat.sum())       # 21
print("Global Mean:", mat.mean())     # 3.5
print("Global StdDev:", mat.std())    # ~1.7078

# axis=0: Collapse rows (computes sum of each COLUMN)
col_sums = mat.sum(axis=0)            # array([5, 7, 9])

# axis=1: Collapse columns (computes sum of each ROW)
row_sums = mat.sum(axis=1)            # array([6, 15])

print("Column sums (axis=0):", col_sums)
print("Row sums (axis=1):", row_sums)
```

#### Aggregate Functions Quick-Reference

| Function | Operation | Example |
| :--- | :--- | :--- |
| `np.sum(a)` | Sum of array elements | `mat.sum(axis=0)` |
| `np.mean(a)` | Arithmetic mean | `mat.mean(axis=1)` |
| `np.max(a)` / `np.min(a)` | Maximum / Minimum values | `mat.max()` |
| `np.std(a)` / `np.var(a)` | Standard deviation / Variance | `mat.std()` |
| `np.argmax(a)` | Index of the maximum value | `predictions.argmax(axis=1)` |

---

### Q10: What Is Array Broadcasting and What Rules Govern Dimension Compatibility?

> **Interview-Ready Answer:**  
> Broadcasting is NumPy's mechanism allowing arithmetic operations between arrays of different shapes without physically replicating data; two dimensions are compatible if they are equal or if one of them is $1$, evaluated from right-to-left trailing dimensions.

#### The Official NumPy Broadcasting Rules:
1. If the arrays differ in dimension count (`ndim`), prepend the shape of the lower-dimensional array with $1$s on the left.
2. Two dimensions are compatible if:
   - They are equal, OR
   - One of them is $1$.
3. When either dimension is $1$, that dimension is stretched (broadcast) along that axis to match the larger size without copying memory.

#### Code Demonstration

```python
import numpy as np

# Shape (3, 3)
A = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

# Shape (3,) -> treated as (1, 3) -> broadcast down the 3 rows
B = np.array([10, 20, 30])

# Broadcasting addition
result = A + B
print("Broadcasting Addition:\n", result)
# [[11, 22, 33],
#  [14, 25, 36],
#  [17, 28, 39]]

# Subtract column mean from each feature (Standardization)
col_mean = A.mean(axis=0)  # Shape (3,)
zero_centered = A - col_mean
print("Zero-centered:\n", zero_centered)
```