# Product of Array Except Self

**LeetCode Problem:** https://leetcode.com/problems/product-of-array-except-self/

## Problem Statement

Given an integer array `nums`, return an array `answer` such that:

```text
answer[i] = product of all elements in nums except nums[i]
```

### Constraints

- You must solve it in **O(n)** time.
- You **cannot use division**.
- Follow-up: Can you solve it using **O(1)** extra space? (excluding the output array) :contentReference[oaicite:1]{index=1}

---

## Example 1

### Input

```text
nums = [1,2,3,4]
```

### Output

```text
[24,12,8,6]
```

### Explanation

```text
Index 0 → 2 × 3 × 4 = 24
Index 1 → 1 × 3 × 4 = 12
Index 2 → 1 × 2 × 4 = 8
Index 3 → 1 × 2 × 3 = 6
```

---

## Example 2

### Input

```text
nums = [-1,1,0,-3,3]
```

### Output

```text
[0,0,9,0,0]
```

---

# Intuition

For every index `i`, we need:

```text
(product of all elements to the left of i)
×
(product of all elements to the right of i)
```

Example:

```text
nums = [1,2,3,4]
```

For index `2`:

```text
Left Product  = 1 × 2 = 2
Right Product = 4

Answer = 2 × 4 = 8
```

Therefore:

```text
answer[i]
=
(prefix product before i)
×
(suffix product after i)
```

This is the key insight behind the optimal solution. :contentReference[oaicite:2]{index=2}

---

# Approach: Prefix Product + Suffix Product

Instead of calculating products repeatedly, we compute:

```text
Prefix Product  → Product of all elements before i
Suffix Product  → Product of all elements after i
```

---

## Step 1: Build Prefix Products

For:

```text
nums = [1,2,3,4]
```

Create:

```text
prefix = [1,1,2,6]
```

Explanation:

```text
prefix[0] = 1

prefix[1] = 1

prefix[2] = 1 × 2 = 2

prefix[3] = 1 × 2 × 3 = 6
```

Each position stores the product of everything to its left.

---

## Step 2: Traverse From Right

Maintain a running suffix product.

Initially:

```text
suffix = 1
```

Traverse from right to left.

### Index 3

```text
answer[3] = prefix[3] × 1
          = 6

suffix = suffix × nums[3]
       = 4
```

### Index 2

```text
answer[2] = prefix[2] × suffix
          = 2 × 4
          = 8

suffix = 4 × 3 = 12
```

### Index 1

```text
answer[1] = 1 × 12
          = 12

suffix = 12 × 2 = 24
```

### Index 0

```text
answer[0] = 1 × 24
          = 24
```

Final Answer:

```text
[24,12,8,6]
```

---

# Visualization

```text
nums:

[1, 2, 3, 4]

Prefix Products:

[1, 1, 2, 6]

Suffix Products:

[24, 12, 4, 1]

Multiply:

1×24 = 24
1×12 = 12
2×4  = 8
6×1  = 6

Answer:

[24,12,8,6]
```

---

# Why Not Use Division?

A simple idea is:

```text
total_product / nums[i]
```

However:

1. The problem explicitly forbids division.
2. Arrays containing zeroes break this approach.

Example:

```text
[1,2,0,4]
```

Total product becomes:

```text
0
```

and division no longer works correctly.

Therefore, we use prefix and suffix products instead. :contentReference[oaicite:3]{index=3}

---

# Complexity Analysis

## Time Complexity

First pass:

```text
O(n)
```

Second pass:

```text
O(n)
```

Total:

```text
O(n)
```

---

## Space Complexity

Ignoring the output array:

```text
O(1)
```

We only use:

```text
prefix product variable
suffix product variable
```

This satisfies the follow-up requirement. :contentReference[oaicite:4]{index=4}

---

# Key Takeaway

The important pattern is:

```text
Prefix Product + Suffix Product
```

Whenever a problem asks:

```text
Use information from all elements before i
and
all elements after i
```

think about:

```text
Prefix Arrays
Suffix Arrays
```

This pattern also appears in:

- Trapping Rain Water
- Prefix Sum Problems
- Range Query Problems
- Product of Array Except Self

A common interview insight is:

> Product Except Self is essentially a prefix/suffix aggregate problem where each answer is built from information on the left and right of the current index. :contentReference[oaicite:5]{index=5}

---

# Python

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:

        n = len(nums)

        result = [1] * n

        prefix = 1

        for i in range(n):
            result[i] = prefix
            prefix *= nums[i]

        suffix = 1

        for i in range(n - 1, -1, -1):
            result[i] *= suffix
            suffix *= nums[i]

        return result
```

---

# Java

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {

        int n = nums.length;

        int[] result = new int[n];

        int prefix = 1;

        for (int i = 0; i < n; i++) {
            result[i] = prefix;
            prefix *= nums[i];
        }

        int suffix = 1;

        for (int i = n - 1; i >= 0; i--) {
            result[i] *= suffix;
            suffix *= nums[i];
        }

        return result;
    }
}
```

---

# C++

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {

        int n = nums.size();

        vector<int> result(n, 1);

        int prefix = 1;

        for (int i = 0; i < n; i++) {
            result[i] = prefix;
            prefix *= nums[i];
        }

        int suffix = 1;

        for (int i = n - 1; i >= 0; i--) {
            result[i] *= suffix;
            suffix *= nums[i];
        }

        return result;
    }
};
```

---

## Final Takeaway

Whenever you need:

```text
Answer at index i
=
Information from the left
+
Information from the right
```

consider using:

```text
Prefix Arrays
Suffix Arrays
```

For this problem:

```text
answer[i]
=
(product of elements before i)
×
(product of elements after i)
```

which leads to the optimal solution:

```text
Time Complexity  : O(n)
Space Complexity : O(1) extra space
```
