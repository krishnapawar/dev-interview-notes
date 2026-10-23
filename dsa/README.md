# 🐘 PHP Interview Solutions — `code.md`

## 1. Sum of Numbers Inside a String

**Input:**
```php
$str = "a2ft4k5";
```

Digits are: `2, 4, 5`
**Sum = 11**

### ✅ Simple PHP Solution
```php
<?php

$str = "a2ft4k5";

preg_match_all('/\d/', $str, $matches);

$sum = array_sum($matches[0]);

echo $sum;  // 11
```

### 🔎 Explanation
- `\d` → matches digits (0–9)
- `preg_match_all()` → extracts all digits
- `array_sum()` → adds them

### ✅ Shorter Version (Clean & Easy)
```php
<?php

$str = "a2ft4k5";

$sum = array_sum(str_split(preg_replace('/\D/', '', $str)));

echo $sum;  // 11
```

- `\D` → removes everything except digits
- `str_split()` → converts string to array
- `array_sum()` → sums digits

> If you want to handle multi-digit numbers like `"a12ft4k15"` (treating `12` and `15` as whole numbers, not `1+2`), the solution is slightly different.

---

## 2. First Non-Repeating Element in an Array

**Example:**
```php
$arr = [4, 5, 1, 2, 0, 4, 1, 2];
```

Non-repeating element is: `5`

### ✅ Method 1 (Best & Clean – Using `array_count_values()`)
```php
<?php

$arr = [4, 5, 1, 2, 0, 4, 1, 2];

$counts = array_count_values($arr);

foreach ($arr as $value) {
    if ($counts[$value] == 1) {
        echo $value;
        break;
    }
}
```

**Output:**
```
5
```

### ✅ Method 2 (Manual Way – Without Built-in Count Function)
```php
<?php

$arr = [4, 5, 1, 2, 0, 4, 1, 2];

$freq = [];

foreach ($arr as $num) {
    if (isset($freq[$num])) {
        $freq[$num]++;
    } else {
        $freq[$num] = 1;
    }
}

foreach ($arr as $num) {
    if ($freq[$num] == 1) {
        echo $num;
        break;
    }
}
```

### ⏱ Time Complexity
**O(n)** — one pass to count + one pass to find.

> For the first non-repeating character in a string, the same approach applies.

---

## 3. Convert Associative Array to a Single String

**Input:**
```php
[A => 2, B => 1, C => 4]
```

**Output:**
```
A2B1C4
```

### ✅ Simple PHP Solution
```php
<?php

$arr = ["A" => 2, "B" => 1, "C" => 4];

$result = "";

foreach ($arr as $key => $value) {
    $result .= $key . $value;
}

echo $result;  // A2B1C4
```

### ✅ One-Line Solution (Using `array_map`)
```php
<?php

$arr = ["A" => 2, "B" => 1, "C" => 4];

echo implode('', array_map(
    fn($k, $v) => $k . $v,
    array_keys($arr),
    $arr
));
```

### 🔎 Explanation
- Loop through array
- Concatenate key + value
- Combine everything into one string

> The reverse (convert `"A2B1C4"` back into an array) is also possible.

---

## 4. Rotate an Array

Rotating an array means shifting elements left or right.

**Example:**
```
Input:   [1,2,3,4,5]
Left 2:  [3,4,5,1,2]
Right 2: [4,5,1,2,3]
```

### ✅ 1️⃣ Left Rotate (PHP)
```php
<?php

$arr = [1,2,3,4,5];
$k = 2; // rotate by 2

$k = $k % count($arr); // handle k > length

$result = array_merge(
    array_slice($arr, $k),
    array_slice($arr, 0, $k)
);

print_r($result);
```

**Output:**
```
[3,4,5,1,2]
```

### ✅ 2️⃣ Right Rotate (PHP)
```php
<?php

$arr = [1,2,3,4,5];
$k = 2;

$k = $k % count($arr);

$result = array_merge(
    array_slice($arr, -$k),
    array_slice($arr, 0, count($arr) - $k)
);

print_r($result);
```

**Output:**
```
[4,5,1,2,3]
```

### ✅ 3️⃣ Manual Method (Without `array_slice`)
```php
<?php

$arr = [1,2,3,4,5];
$k = 2;

for ($i = 0; $i < $k; $i++) {
    $first = array_shift($arr);
    array_push($arr, $first);
}

print_r($arr);
```

### ⏱ Time Complexity
- Using slice → **O(n)**
- Using shift/push → **O(n × k)**

---

## 5. Output Order: Timeout, Promise, and Normal Console (JavaScript Event Loop)

This is about **JavaScript execution order (Event Loop)** — a very common interview question.

### ✅ Example Code
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

### ❓ What will be the Output?
```
1
4
3
2
```

### 🔍 Explanation (Step-by-Step)

**🔹 1. Synchronous code (runs first)**
- `console.log("1")` → prints `1`
- `console.log("4")` → prints `4`

**🔹 2. Microtasks (Promises)**
- `Promise.then` → goes to **Microtask Queue**
- 👉 Microtasks run after sync code but **before** timers
- So: `3`

**🔹 3. Macrotasks (setTimeout)**
- `setTimeout` → goes to **Callback Queue (Macrotask)**
- Runs last: `2`

### 🧠 Final Order

| Type | Example | Priority |
|------|---------|----------|
| Sync | `console.log` | 1st |
| Microtask | `Promise.then` | 2nd |
| Macrotask | `setTimeout` | 3rd |

### ⚡ Rule to Remember
> **Sync → Microtask (Promise) → Macrotask (setTimeout)**

### 🔥 Another Example (Tricky)
```javascript
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

**Output:**
```
A
D
C
B
```

---

## 6. Convert JavaScript `isAnagram` Function to PHP

**Original JavaScript:**
```javascript
function isAnagram(str1, str2) {
    let sortStr = s => s.split('').sort().join('');
    return sortStr(str1) === sortStr(str2);
}

console.log(isAnagram("listen", "silent")); // true
```

### ✅ PHP Version
```php
<?php

function isAnagram($str1, $str2) {
    // Convert string to array, sort, then join back
    $arr1 = str_split($str1);
    $arr2 = str_split($str2);

    sort($arr1);
    sort($arr2);

    return implode('', $arr1) === implode('', $arr2);
}

// Example
var_dump(isAnagram("listen", "silent")); // true
```

### 🔍 Explanation
- `str_split()` → converts string to array of characters
- `sort()` → sorts characters alphabetically
- `implode()` → joins back to string
- Compare both sorted strings

### ✅ Bonus (Case-Insensitive + Ignore Spaces)
```php
<?php

function isAnagram($str1, $str2) {
    $str1 = strtolower(str_replace(' ', '', $str1));
    $str2 = strtolower(str_replace(' ', '', $str2));

    $arr1 = str_split($str1);
    $arr2 = str_split($str2);

    sort($arr1);
    sort($arr2);

    return implode('', $arr1) === implode('', $arr2);
}

var_dump(isAnagram("Listen", "Silent")); // true
```

> A more optimized approach uses **frequency count (O(n))** instead of sorting **(O(n log n))**.