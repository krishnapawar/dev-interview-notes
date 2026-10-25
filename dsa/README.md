# 🧮 Data Structures & Algorithms — Interview Questions & Answers

A comprehensive collection of core Data Structures & Algorithms interview questions, algorithmic problem-solving patterns, and high-frequency technical solutions in PHP and JavaScript.

---

## 📋 Table of Contents

1. [How to Calculate the Sum of Numbers Inside a String?](#q1-how-to-calculate-the-sum-of-numbers-inside-a-string)
2. [How to Find the First Non-Repeating Element in an Array?](#q2-how-to-find-the-first-non-repeating-element-in-an-array)
3. [How to Convert an Associative Array or Key-Value Map to a Single String?](#q3-how-to-convert-an-associative-array-or-key-value-map-to-a-single-string)
4. [How to Rotate an Array Left or Right by K Positions?](#q4-how-to-rotate-an-array-left-or-right-by-k-positions)
5. [Explain the Execution Order of Synchronous Code, Promises, and setTimeout (Event Loop)](#q5-explain-the-execution-order-of-synchronous-code-promises-and-settimeout-event-loop)
6. [How to Check if Two Strings Are Anagrams?](#q6-how-to-check-if-two-strings-are-anagrams)
7. [How to Find the Largest Number in an Array?](#q7-how-to-find-the-largest-number-in-an-array)
8. [How to Count Character Frequencies and Format as a Compressed String?](#q8-how-to-count-character-frequencies-and-format-as-a-compressed-string)
9. [How to Find the Highest Value Grouped by Property in an Array of Objects?](#q9-how-to-find-the-highest-value-grouped-by-property-in-an-array-of-objects)
10. [How to Find Maximum Value by Department and Update Salaries via JOIN in SQL?](#q10-how-to-find-maximum-value-by-department-and-update-salaries-via-join-in-sql)
11. [How to Calculate Percentage Increase and Decrease in Numerical Queries?](#q11-how-to-calculate-percentage-increase-and-decrease-in-numerical-queries)
12. [How to Find the Missing Number in an Array from 0 to N?](#q12-how-to-find-the-missing-number-in-an-array-from-0-to-n)
13. [How Does Bubble Sort Work and How Do You Optimize It?](#q13-how-does-bubble-sort-work-and-how-do-you-optimize-it)
14. [How to Move All Zeros to the End of an Array While Maintaining Relative Order?](#q14-how-to-move-all-zeros-to-the-end-of-an-array-while-maintaining-relative-order)
15. [How to Remove or Extract All Digits from a String Using Regular Expressions?](#q15-how-to-remove-or-extract-all-digits-from-a-string-using-regular-expressions)

---

### Q1: How to Calculate the Sum of Numbers Inside a String?

> **Interview-Ready Answer:**  
> Extract all digit characters from the string using a regular expression (`/\d/` for individual digits or `/\d+/` for multi-digit numbers), convert the matches to numbers, and compute their total sum in $O(N)$ time.

#### Example
```text
Input:  "a2ft4k5"
Digits: 2, 4, 5
Output: 11
```

#### Solution 1: PHP (Using `preg_match_all`)
```php
<?php

$str = "a2ft4k5";

// Extract all individual digits
preg_match_all('/\d/', $str, $matches);

// Sum the matched values
$sum = array_sum($matches[0]);

echo $sum; // Output: 11
```

#### Solution 2: PHP Clean One-Liner (Using `preg_replace` + `str_split`)
```php
<?php

$str = "a2ft4k5";

// Remove all non-digits (\D), split into characters, and sum
$sum = array_sum(str_split(preg_replace('/\D/', '', $str)));

echo $sum; // Output: 11
```

#### Solution 3: JavaScript Implementation
```javascript
const str = "a2ft4k5";

// Match all single digits and sum using reduce
const sum = (str.match(/\d/g) || [])
    .reduce((acc, digit) => acc + Number(digit), 0);

console.log(sum); // Output: 11
```

#### 🔹 Handling Multi-Digit Numbers
If the string contains numbers with more than one digit (e.g., `"a12ft4k15"` where the numbers are `12`, `4`, and `15`):

```php
<?php

$str = "a12ft4k15";

// Match one or more contiguous digits (\d+)
preg_match_all('/\d+/', $str, $matches);

$sum = array_sum($matches[0]);

echo $sum; // Output: 31 (12 + 4 + 15)
```

#### ⏱ Complexity Analysis
- **Time Complexity:** $O(N)$ — Single pass scanning through the string of length $N$.
- **Space Complexity:** $O(M)$ — Memory to store matched numbers where $M \le N$.

---

### Q2: How to Find the First Non-Repeating Element in an Array?

> **Interview-Ready Answer:**  
> Build a frequency map in a first pass $O(N)$ to count occurrences of each element, then iterate through the array in a second pass $O(N)$ to return the first element with a frequency count of 1.

#### Example
```text
Input:  [4, 5, 1, 2, 0, 4, 1, 2]
Output: 5
```

#### Method 1: Using Built-in Frequency Counting (`array_count_values`)
```php
<?php

$arr = [4, 5, 1, 2, 0, 4, 1, 2];

// Count frequency of all elements
$counts = array_count_values($arr);

// Find first element with count === 1
$firstUnique = null;
foreach ($arr as $value) {
    if ($counts[$value] === 1) {
        $firstUnique = $value;
        break;
    }
}

echo $firstUnique; // Output: 5
```

#### Method 2: Manual Frequency Map (Without Built-in Helpers)
```php
<?php

function findFirstNonRepeating(array $arr) {
    $freq = [];

    // Pass 1: Build frequency map
    foreach ($arr as $num) {
        $freq[$num] = ($freq[$num] ?? 0) + 1;
    }

    // Pass 2: Identify first element with frequency 1
    foreach ($arr as $num) {
        if ($freq[$num] === 1) {
            return $num;
        }
    }

    return null; // All elements repeat or array is empty
}

$arr = [4, 5, 1, 2, 0, 4, 1, 2];
echo findFirstNonRepeating($arr); // Output: 5
```

#### Method 3: JavaScript Implementation (Using `Map`)
```javascript
function findFirstNonRepeating(arr) {
    const freq = new Map();

    for (const item of arr) {
        freq.set(item, (freq.get(item) || 0) + 1);
    }

    for (const item of arr) {
        if (freq.get(item) === 1) {
            return item;
        }
    }

    return null;
}

console.log(findFirstNonRepeating([4, 5, 1, 2, 0, 4, 1, 2])); // Output: 5
```

#### ⏱ Complexity Analysis
- **Time Complexity:** $O(N)$ — Two sequential passes through the array.
- **Space Complexity:** $O(U)$ — Storing frequency counts where $U$ is the number of unique elements.

---

### Q3: How to Convert an Associative Array or Key-Value Map to a Single String?

> **Interview-Ready Answer:**  
> Iterate over key-value entries concatenating each key and its corresponding value into a string buffer, or use functional mapping (`array_map` with `implode` in PHP, `Object.entries` with `map` in JavaScript) in $O(N)$ time.

#### Example
```text
Input:  ["A" => 2, "B" => 1, "C" => 4]
Output: "A2B1C4"
```

#### Solution 1: Iterative Concatenation (PHP)
```php
<?php

$arr = ["A" => 2, "B" => 1, "C" => 4];
$result = "";

foreach ($arr as $key => $value) {
    $result .= $key . $value;
}

echo $result; // Output: "A2B1C4"
```

#### Solution 2: Functional One-Liner (PHP)
```php
<?php

$arr = ["A" => 2, "B" => 1, "C" => 4];

$result = implode('', array_map(
    fn($k, $v) => $k . $v,
    array_keys($arr),
    $arr
));

echo $result; // Output: "A2B1C4"
```

#### Solution 3: JavaScript Implementation
```javascript
const obj = { A: 2, B: 1, C: 4 };

const result = Object.entries(obj)
    .map(([key, val]) => `${key}${val}`)
    .join('');

console.log(result); // Output: "A2B1C4"
```

#### 🔹 Reverse Operation: String Back to Key-Value Map
```javascript
const str = "A2B1C4";
const parsed = {};

// Regex matches letter followed by one or more digits
const matches = str.matchAll(/([A-Za-z]+)(\d+)/g);
for (const match of matches) {
    parsed[match[1]] = Number(match[2]);
}

console.log(parsed); // Output: { A: 2, B: 1, C: 4 }
```

#### ⏱ Complexity Analysis
- **Time Complexity:** $O(N)$ — Iterating across $N$ key-value pairs.
- **Space Complexity:** $O(N)$ — To store the final concatenated output string.

---

### Q4: How to Rotate an Array Left or Right by K Positions?

> **Interview-Ready Answer:**  
> An array can be rotated in $O(N)$ time using slicing operations normalized with `k % length`, or in $O(N)$ time and $O(1)$ auxiliary space using the three-step in-place reversal algorithm.

#### Example
```text
Input:       [1, 2, 3, 4, 5], k = 2
Left Rotate:  [3, 4, 5, 1, 2]
Right Rotate: [4, 5, 1, 2, 3]
```

#### Method 1: Using Slicing (PHP)
```php
<?php

$arr = [1, 2, 3, 4, 5];
$k = 2;

// Normalize k to prevent unnecessary full rotations
$k = $k % count($arr);

// Left Rotate: Shift front elements to back
$leftRotated = array_merge(
    array_slice($arr, $k),
    array_slice($arr, 0, $k)
);

// Right Rotate: Shift end elements to front
$rightRotated = array_merge(
    array_slice($arr, -$k),
    array_slice($arr, 0, count($arr) - $k)
);

print_r($leftRotated);  // [3, 4, 5, 1, 2]
print_r($rightRotated); // [4, 5, 1, 2, 3]
```

#### Method 2: In-Place Reversal Algorithm (Optimal $O(1)$ Auxiliary Space)
To right-rotate an array by $K$ positions:
1. Reverse the entire array.
2. Reverse the first $K$ elements.
3. Reverse the remaining $N - K$ elements.

```javascript
function reverse(arr, start, end) {
    while (start < end) {
        [arr[start], arr[end]] = [arr[end], arr[start]];
        start++;
        end--;
    }
}

function rotateRight(arr, k) {
    const n = arr.length;
    k = k % n;

    reverse(arr, 0, n - 1); // Reverse whole array
    reverse(arr, 0, k - 1); // Reverse first k elements
    reverse(arr, k, n - 1); // Reverse remainder

    return arr;
}

console.log(rotateRight([1, 2, 3, 4, 5], 2)); // [4, 5, 1, 2, 3]
```

#### Comparison of Approaches

| Approach | Time Complexity | Auxiliary Space | Best Use Case |
| :--- | :--- | :--- | :--- |
| **Slice & Merge** | $O(N)$ | $O(N)$ | Concise scripting when extra memory is allowed |
| **In-Place Reversal** | $O(N)$ | $O(1)$ | Memory-constrained systems, competitive coding |
| **Shift / Push Loop** | $O(N \times K)$ | $O(1)$ | Small arrays only; inefficient for large $K$ |

---

### Q5: Explain the Execution Order of Synchronous Code, Promises, and setTimeout (Event Loop)

> **Interview-Ready Answer:**  
> The JavaScript runtime processes all synchronous code on the Call Stack first, drains the Microtask Queue (Promises, `queueMicrotask`) until empty, and only then dequeues Macrotasks (`setTimeout`, `setInterval`) from the Callback Queue.

#### Code Example
```javascript
console.log("1");

setTimeout(() => {
    console.log("2");
}, 0);

Promise.resolve().then(() => {
    console.log("3");
});

console.log("4");
```

#### ❓ Output
```text
1
4
3
2
```

#### 🔍 Step-by-Step Execution Breakdown

1. **Synchronous Execution:**
   - `console.log("1")` pushes to Call Stack and prints `1`.
   - `setTimeout(..., 0)` registers a timer with the Web API environment; its callback is queued in the **Macrotask Queue**.
   - `Promise.resolve().then(...)` queues its resolution callback into the **Microtask Queue**.
   - `console.log("4")` pushes to Call Stack and prints `4`.
2. **Microtask Queue Draining:**
   - Call Stack is now empty. The Event Loop prioritizes the **Microtask Queue** before rendering or macrotasks.
   - The Promise callback executes and prints `3`.
3. **Macrotask Execution:**
   - The Event Loop picks the oldest task from the **Macrotask Queue**.
   - The `setTimeout` callback executes and prints `2`.

#### 🧠 Event Loop Task Priority Table

| Queue Type | Typical APIs | Priority Order |
| :--- | :--- | :--- |
| **Call Stack** | Synchronous statements, function calls | **1st** (Executes immediately) |
| **Microtask Queue** | `Promise.then`, `catch`, `finally`, `queueMicrotask`, `process.nextTick` | **2nd** (Runs immediately when Call Stack empties) |
| **Macrotask Queue** | `setTimeout`, `setInterval`, `setImmediate`, I/O operations | **3rd** (Runs one task per event loop tick) |

#### ⚡ Rule to Remember
> **Synchronous $\rightarrow$ Microtasks (Promise) $\rightarrow$ Macrotasks (setTimeout)**

---

### Q6: How to Check if Two Strings Are Anagrams?

> **Interview-Ready Answer:**  
> Two strings are anagrams if they contain the identical set of characters with the same frequencies. This can be verified by sorting both strings in $O(N \log N)$ or comparing character counts via a frequency hash map in $O(N)$ time.

#### Example
```text
Input:  "listen", "silent" -> true
Input:  "hello", "world"   -> false
```

#### Method 1: Sorting Approach
```php
<?php

function isAnagramSort(string $str1, string $str2): bool {
    // Normalize: remove spaces and convert to lowercase
    $s1 = strtolower(str_replace(' ', '', $str1));
    $s2 = strtolower(str_replace(' ', '', $str2));

    if (strlen($s1) !== strlen($s2)) {
        return false;
    }

    $arr1 = str_split($s1);
    $arr2 = str_split($s2);

    sort($arr1);
    sort($arr2);

    return implode('', $arr1) === implode('', $arr2);
}

var_dump(isAnagramSort("listen", "silent")); // bool(true)
var_dump(isAnagramSort("Hello", "Ohell"));   // bool(true)
```

#### Method 2: Frequency Map Approach (Optimal $O(N)$ Time)
```javascript
function isAnagram(str1, str2) {
    const s1 = str1.replace(/\s+/g, '').toLowerCase();
    const s2 = str2.replace(/\s+/g, '').toLowerCase();

    if (s1.length !== s2.length) return false;

    const charCount = {};

    for (const char of s1) {
        charCount[char] = (charCount[char] || 0) + 1;
    }

    for (const char of s2) {
        if (!charCount[char]) {
            return false;
        }
        charCount[char]--;
    }

    return true;
}

console.log(isAnagram("listen", "silent")); // true
console.log(isAnagram("rat", "car"));       // false
```

#### ⏱ Complexity Analysis
- **Sorting Approach:** $O(N \log N)$ Time, $O(N)$ Space.
- **Frequency Map Approach:** $O(N)$ Time, $O(K)$ Space (where $K \le 26$ for lowercase English letters).

---

### Q7: How to Find the Largest Number in an Array?

> **Interview-Ready Answer:**  
> Traverse the array while keeping track of a running maximum initialized to the first element in $O(N)$ time and $O(1)$ space, or use built-in functions like `max()` or `Math.max()`.

#### Method 1: Built-in Approach
```php
<?php

function findLargest(array $arr) {
    if (empty($arr)) {
        return null;
    }
    return max($arr);
}

$numbers = [10, 5, 8, 20, 3];
echo findLargest($numbers); // Output: 20
```

#### Method 2: Manual Iteration (Safe for Custom Constraints)
```javascript
function findLargestNumber(arr) {
    if (!arr || arr.length === 0) {
        return null;
    }

    let maxVal = arr[0];

    for (let i = 1; i < arr.length; i++) {
        if (arr[i] > maxVal) {
            maxVal = arr[i];
        }
    }

    return maxVal;
}

console.log(findLargestNumber([10, 5, 8, 20, 3])); // Output: 20
console.log(findLargestNumber([-15, -3, -22, -1])); // Output: -1
```

#### ⚠️ Edge Cases to Handle in Interviews
- **Empty Array:** Return `null` or throw an `InvalidArgumentException`.
- **Negative Numbers:** Do not initialize `max = 0`; initialize with `arr[0]` or `-Infinity`.
- **Duplicates:** Strict inequality (`>`) properly handles arrays with repeated maximum values.

---

### Q8: How to Count Character Frequencies and Format as a Compressed String?

> **Interview-Ready Answer:**  
> Iterate through the string to aggregate character counts into a hash map in $O(N)$ time, then serialize each entry into `character + count` format and join them into a compact string.

#### Example
```text
Input:  "A lump-sum payment"
Output: "A1 2l1u2m2p2-1s1a2y1e1n1t1" (or filtered letters only)
```

#### JavaScript Solution
```javascript
function compressCharacterFrequencies(str, lettersOnly = false) {
    const counts = {};

    // Filter characters if requested
    const target = lettersOnly ? str.replace(/[^a-zA-Z0-9]/g, '') : str;

    for (const char of target) {
        counts[char] = (counts[char] || 0) + 1;
    }

    // Convert map to "A2l4u5..." string
    return Object.entries(counts)
        .map(([char, count]) => `${char}${count}`)
        .join('');
}

const text = "A lump-sum payment";
console.log(compressCharacterFrequencies(text, true));
// Output: "A1l1u2m2p2a2y1e1n1t1"
```

#### PHP Solution
```php
<?php

function compressCharFrequencies(string $str): string {
    $counts = [];
    $len = strlen($str);

    for ($i = 0; $i < $len; $i++) {
        $char = $str[$i];
        $counts[$char] = ($counts[$char] ?? 0) + 1;
    }

    $output = "";
    foreach ($counts as $char => $count) {
        $output .= $char . $count;
    }

    return $output;
}

echo compressCharFrequencies("aabcccccaaa"); // Output: "a5b1c5"
```

#### ⏱ Complexity Analysis
- **Time Complexity:** $O(N)$ — Single pass to build frequency map and single pass over unique keys.
- **Space Complexity:** $O(U)$ — Map storage where $U$ is the number of distinct characters in the input.

---

### Q9: How to Find the Highest Value Grouped by Property in an Array of Objects?

> **Interview-Ready Answer:**  
> Iterate over the array of objects while maintaining an accumulator object keyed by the grouping attribute (e.g. `department` or `designation`), replacing the stored object whenever a higher numeric value is encountered.

#### Example Dataset
```javascript
const employees = [
    { id: 1, name: "John", salary: 123, designation: "HR" },
    { id: 2, name: "Ranu", salary: 234, designation: "HR" },
    { id: 3, name: "Manish", salary: 333, designation: "HR" },
    { id: 4, name: "Mohan", salary: 555, designation: "Developer" },
    { id: 5, name: "Ram", salary: 1568, designation: "Developer" }
];
```

#### JavaScript Solution
```javascript
function getHighestByDesignation(records) {
    const highestByRole = {};

    for (const person of records) {
        const role = person.designation;

        if (!highestByRole[role] || person.salary > highestByRole[role].salary) {
            highestByRole[role] = person;
        }
    }

    return highestByRole;
}

console.log(getHighestByDesignation(employees));
/* Output:
{
  HR: { id: 3, name: 'Manish', salary: 333, designation: 'HR' },
  Developer: { id: 5, name: 'Ram', salary: 1568, designation: 'Developer' }
}
*/
```

#### PHP Solution
```php
<?php

$employees = [
    ["id" => 1, "name" => "John", "salary" => 123, "designation" => "HR"],
    ["id" => 2, "name" => "Ranu", "salary" => 234, "designation" => "HR"],
    ["id" => 3, "name" => "Manish", "salary" => 333, "designation" => "HR"],
    ["id" => 4, "name" => "Mohan", "salary" => 555, "designation" => "Developer"],
    ["id" => 5, "name" => "Ram", "salary" => 1568, "designation" => "Developer"]
];

$highestByRole = [];

foreach ($employees as $person) {
    $role = $person['designation'];

    if (!isset($highestByRole[$role]) || $person['salary'] > $highestByRole[$role]['salary']) {
        $highestByRole[$role] = $person;
    }
}

print_r($highestByRole);
```

#### ⏱ Complexity Analysis
- **Time Complexity:** $O(N)$ — Single pass over $N$ employee records.
- **Space Complexity:** $O(K)$ — Auxiliary hash map storing $K$ unique designations.

---

### Q10: How to Find Maximum Value by Department and Update Salaries via JOIN in SQL?

> **Interview-Ready Answer:**  
> Use `SELECT ... GROUP BY` with the `MAX()` aggregate function to retrieve top metrics per group, and use MySQL `UPDATE ... JOIN` syntax to apply batch modifications across joined relational tables.

#### Scenario
Two tables: `empl` (`id`, `name`, `salary`, `dept_id`) and `depart` (`id`, `deptname`).

#### Task 1: Find the Highest Salary in Each Department
```sql
SELECT 
    d.deptname, 
    MAX(e.salary) AS max_salary
FROM empl e
JOIN depart d ON d.id = e.dept_id
GROUP BY d.deptname;
```

#### Task 2: Increase Salary by 10% for All Employees in the HR Department
```sql
UPDATE empl e
JOIN depart d ON d.id = e.dept_id
SET e.salary = e.salary * 1.10
WHERE d.deptname = 'HR';
```

#### ⚠️ Common SQL Syntax Mistakes to Avoid

| ❌ Incorrect Syntax | ✅ Correct Syntax | Cause of Error |
| :--- | :--- | :--- |
| `setect depart.deptname ... form empl` | `SELECT d.deptname ... FROM empl e` | Typo in keyword names (`SETECT`, `FORM`) |
| `UPDATE empl FROM empl JOIN depart ...` | `UPDATE empl e JOIN depart d ...` | Redundant `FROM` clause in MySQL `UPDATE` |
| `WHERE depart.deptname = "HR"` without JOIN | `JOIN depart d ON d.id = e.dept_id WHERE ...` | Cannot reference joined columns without explicit join clause |

---

### Q11: How to Calculate Percentage Increase and Decrease in Numerical Queries?

> **Interview-Ready Answer:**  
> To increase a value by $X\%$, multiply the base value by $(1 + X/100)$; to decrease by $X\%$, multiply the base value by $(1 - X/100)$. This ensures relative scaling proportional to each item's current amount.

#### Mathematical Formulas
- **Percentage Increase:**  
  $$\text{New Value} = \text{Old Value} \times \left(1 + \frac{X}{100}\right)$$
- **Percentage Decrease:**  
  $$\text{New Value} = \text{Old Value} \times \left(1 - \frac{X}{100}\right)$$

#### SQL Examples

```sql
-- Increase HR department salaries by 10%
UPDATE empl e
JOIN depart d ON d.id = e.dept_id
SET e.salary = e.salary * 1.10
WHERE d.deptname = 'HR';

-- Decrease HR department salaries by 10%
UPDATE empl e
JOIN depart d ON d.id = e.dept_id
SET e.salary = e.salary * 0.90
WHERE d.deptname = 'HR';
```

#### Multiplier Reference Table

| Target Percentage Change | Multiplier | Example ($1,000 Base) |
| :--- | :--- | :--- |
| **+5% Increase** | `* 1.05` | $1,050 |
| **+10% Increase** | `* 1.10` | $1,100 |
| **+25% Increase** | `* 1.25` | $1,250 |
| **-5% Decrease** | `* 0.95` | $950 |
| **-10% Decrease** | `* 0.90` | $900 |
| **-20% Decrease** | `* 0.80` | $800 |

#### ⚠️ How to Restore Original Value After Increase
If a salary is increased by $10\%$ (`* 1.10`), dividing by $1.10$ restores the exact original value:
```sql
UPDATE empl e
JOIN depart d ON d.id = e.dept_id
SET e.salary = e.salary / 1.10
WHERE d.deptname = 'HR';
```
*(Note: Multiplying by `0.90` will **not** restore the original value due to base difference).*

---

### Q12: How to Find the Missing Number in an Array from 0 to N?

> **Interview-Ready Answer:**  
> Sum all expected integers from $0$ to $N$ using Gauss's formula $\frac{N(N+1)}{2}$ and subtract the array sum in $O(N)$ time and $O(1)$ space, or compute the XOR of all indices and array values to avoid integer overflow.

#### Example
```text
Input:  [3, 0, 1] (size n = 3, range 0..3)
Output: 2
```

#### Method 1: Mathematical Sum Formula
```javascript
function findMissingNumber(arr) {
    const n = arr.length;
    const expectedSum = (n * (n + 1)) / 2;
    const actualSum = arr.reduce((acc, num) => acc + num, 0);

    return expectedSum - actualSum;
}

console.log(findMissingNumber([3, 0, 1])); // Output: 2
console.log(findMissingNumber([9, 6, 4, 2, 3, 5, 7, 0, 1])); // Output: 8
```

#### Method 2: Bitwise XOR Trick (Safe Against Arithmetic Overflow)
Because $X \oplus X = 0$ and $X \oplus 0 = X$, XORing all numbers from $0$ to $N$ with all array elements cancels out every number appearing in both, leaving only the missing number:

```javascript
function findMissingNumberXOR(arr) {
    const n = arr.length;
    let xor = 0;

    // XOR all expected numbers from 0 to n
    for (let i = 0; i <= n; i++) {
        xor ^= i;
    }

    // XOR all elements in the array
    for (const num of arr) {
        xor ^= num;
    }

    return xor;
}

console.log(findMissingNumberXOR([3, 0, 1])); // Output: 2
```

#### Method 3: PHP Implementation
```php
<?php

function findMissingNumber(array $arr): int {
    $n = count($arr);
    $expectedSum = ($n * ($n + 1)) / 2;
    $actualSum = array_sum($arr);

    return $expectedSum - $actualSum;
}

echo findMissingNumber([3, 0, 1]); // Output: 2
```

#### ⏱ Complexity Analysis
- **Time Complexity:** $O(N)$ — Single iteration through the array.
- **Space Complexity:** $O(1)$ — Only a few primitive accumulator variables.

---

### Q13: How Does Bubble Sort Work and How Do You Optimize It?

> **Interview-Ready Answer:**  
> Bubble Sort repeatedly compares adjacent elements and swaps them if they are in the wrong order, causing the largest unsorted value to "bubble up" to the end of each pass. An `isSwapped` flag optimizes best-case time to $O(N)$ when the array is already sorted.

#### Mechanism Walkthrough
Given array `[5, 1, 4, 2]`:
- Compare `5` & `1` $\rightarrow$ swap $\rightarrow$ `[1, 5, 4, 2]`
- Compare `5` & `4` $\rightarrow$ swap $\rightarrow$ `[1, 4, 5, 2]`
- Compare `5` & `2` $\rightarrow$ swap $\rightarrow$ `[1, 4, 2, 5]` (5 is in its correct final position)
- Repeat for remaining unsorted sub-arrays until no swaps occur.

#### JavaScript Implementation (With Early Termination Optimization)
```javascript
function bubbleSort(arr) {
    const n = arr.length;

    for (let i = 0; i < n - 1; i++) {
        let swapped = false;

        // Last i elements are already sorted
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap adjacent elements
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                swapped = true;
            }
        }

        // If no two elements were swapped in inner loop, array is sorted
        if (!swapped) break;
    }

    return arr;
}

console.log(bubbleSort([5, 1, 4, 2, 8])); // Output: [1, 2, 4, 5, 8]
```

#### PHP Implementation
```php
<?php

function bubbleSort(array &$arr): void {
    $n = count($arr);

    for ($i = 0; $i < $n - 1; $i++) {
        $swapped = false;

        for ($j = 0; $j < $n - $i - 1; $j++) {
            if ($arr[$j] > $arr[$j + 1]) {
                $temp = $arr[$j];
                $arr[$j] = $arr[$j + 1];
                $arr[$j + 1] = $temp;
                $swapped = true;
            }
        }

        if (!$swapped) break;
    }
}

$data = [5, 1, 4, 2, 8];
bubbleSort($data);
print_r($data); // [1, 2, 4, 5, 8]
```

#### ⏱ Complexity Analysis

| Metric | Time Complexity | Notes |
| :--- | :--- | :--- |
| **Best Case** | $O(N)$ | Occurs when input array is already sorted (exits on pass 1) |
| **Average Case** | $O(N^2)$ | Inverted pairs require iterative swapping |
| **Worst Case** | $O(N^2)$ | Occurs when input array is reverse sorted |
| **Auxiliary Space** | $O(1)$ | Sorts completely in-place |

---

### Q14: How to Move All Zeros to the End of an Array While Maintaining Relative Order?

> **Interview-Ready Answer:**  
> Use the two-pointer technique to shift all non-zero elements toward the front of the array in a single pass in $O(N)$ time, then fill the remaining indices with zeros, achieving $O(N)$ time and $O(1)$ auxiliary space without altering relative order.

#### Example
```text
Input:  [0, 0, 1, 0, 3]
Output: [1, 3, 0, 0, 0]
```

#### ⚠️ The Single-Pass Adjacent Swap Pitfall
A common mistake is attempting adjacent element swaps in a single loop:
```javascript
// ❌ WRONG: Single-pass adjacent swap fails with multiple consecutive zeros
let arr = [0, 0, 1, 0, 3];
for (let i = 1; i < arr.length; i++) {
    if (arr[i - 1] === 0) {
        [arr[i - 1], arr[i]] = [arr[i], arr[i - 1]];
    }
}
console.log(arr); // Output: [0, 1, 0, 3, 0] (Incomplete!)
```

#### Optimal Method 1: Two-Pointer Approach ($O(N)$ Time, $O(1)$ Space)
```javascript
function moveZeroes(arr) {
    let nonZeroIndex = 0;

    // Step 1: Shift non-zero elements to the front
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] !== 0) {
            arr[nonZeroIndex++] = arr[i];
        }
    }

    // Step 2: Fill remaining positions with zeros
    while (nonZeroIndex < arr.length) {
        arr[nonZeroIndex++] = 0;
    }

    return arr;
}

console.log(moveZeroes([0, 0, 1, 0, 3])); // Output: [1, 3, 0, 0, 0]
```

#### Method 2: Two-Pointer Swap In-Place (Single Loop)
```javascript
function moveZeroesSwap(arr) {
    let left = 0;

    for (let right = 0; right < arr.length; right++) {
        if (arr[right] !== 0) {
            [arr[left], arr[right]] = [arr[right], arr[left]];
            left++;
        }
    }

    return arr;
}

console.log(moveZeroesSwap([0, 0, 1, 0, 3])); // Output: [1, 3, 0, 0, 0]
```

#### Method 3: PHP Functional Approach
```php
<?php

$arr = [0, 0, 1, 0, 3];

// 1. Filter out all zero values (preserves values)
$nonZero = array_filter($arr, fn($x) => $x !== 0);

// 2. Count number of zeros needed
$zeroCount = count($arr) - count($nonZero);

// 3. Create zeros array and merge
$result = array_merge(array_values($nonZero), array_fill(0, $zeroCount, 0));

print_r($result);
/* Output:
Array
(
    [0] => 1
    [1] => 3
    [2] => 0
    [3] => 0
    [4] => 0
)
*/
```

---

### Q15: How to Remove or Extract All Digits from a String Using Regular Expressions?

> **Interview-Ready Answer:**  
> Use regular expression character classes: `/\d/` matches any numeric digit `0-9` (for removal), while `/\D/` matches any non-digit character (for extracting only numbers), substituting matched patterns with an empty string in $O(N)$ time.

#### Example
```text
Input:              "a2ft4k5"
Remove Digits:      "aftk"
Extract Only Digits:"245"
```

#### Solution in JavaScript
```javascript
const str = "a2ft4k5";

// 1. Remove all digits: match \d (digits) globally
const lettersOnly = str.replace(/\d/g, "");
console.log(lettersOnly); // Output: "aftk"

// 2. Extract only digits: match \D (non-digits) globally
const digitsOnly = str.replace(/\D/g, "");
console.log(digitsOnly); // Output: "245"
```

#### Solution in PHP
```php
<?php

$str = "a2ft4k5";

// 1. Remove all digits
$lettersOnly = preg_replace('/\d/', '', $str);
echo $lettersOnly; // Output: "aftk"

// 2. Extract only digits
$digitsOnly = preg_replace('/\D/', '', $str);
echo $digitsOnly;  // Output: "245"
```

#### 🔍 Regular Expression Tokens Reference

| Token | Character Set Matched | Purpose |
| :--- | :--- | :--- |
| `\d` | `[0-9]` | Matches any digit character |
| `\D` | `[^0-9]` | Matches any character that is NOT a digit |
| `[a-zA-Z]` | Alphabet characters | Matches uppercase and lowercase letters |
| `\s` | `[ \t\r\n\f]` | Matches whitespace characters |
