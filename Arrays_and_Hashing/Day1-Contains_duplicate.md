# Contains Duplicate

## Problem Statement

Given an array of integers, determine whether any value appears more than once.

Return:

- `true` if any element appears at least twice.
- `false` if every element is distinct.

### Example 1

Input:

```text
[1, 2, 3, 1]
```

Output:

```text
true
```

Explanation:

The number `1` appears more than once.

---

### Example 2

Input:

```text
[1, 2, 3, 4]
```

Output:

```text
false
```

Explanation:

All elements are unique.

---

### Example 3

Input:

```text
[1, 1, 1, 3, 3, 4, 3, 2, 4, 2]
```

Output:

```text
true
```

Explanation:

Multiple elements appear more than once.

---

# Understanding the Problem

The goal is not to count how many times each number appears.

We only need to answer a single question:

> Have we seen this element before?

If the answer is yes, then a duplicate exists and we can immediately return `true`.

If we finish checking all elements without finding any repeated value, we return `false`.

---

# Brute Force Approach

Compare every element with every other element.

### Steps

1. Pick an element.
2. Compare it with all remaining elements.
3. If a match is found, return `true`.
4. If no match is found after checking all pairs, return `false`.

### Time Complexity

```text
O(n²)
```

### Space Complexity

```text
O(1)
```

### Why is it inefficient?

For large arrays, repeatedly comparing elements becomes expensive.

Example:

```text
[1, 2, 3, 4, 5, 6, 7, 8]
```

The first element is compared with every other element, then the second element is compared again, and so on.

---

# Optimized Approach Using a Set

A set is a data structure that stores only unique values.

Before adding an element to the set:

- Check whether it already exists.
- If it exists, a duplicate has been found.
- If it does not exist, add it to the set.

---

# Algorithm

1. Create an empty set.
2. Traverse through each element in the array.
3. For every element:
   - If it is already present in the set, return `true`.
   - Otherwise, add it to the set.
4. If the traversal completes, return `false`.

---

# Dry Run

Input:

```text
[1, 2, 3, 1]
```

Initial set:

```text
{}
```

### Step 1

Current element:

```text
1
```

Set:

```text
{}
```

Not found.

Add it.

```text
{1}
```

---

### Step 2

Current element:

```text
2
```

Set:

```text
{1}
```

Not found.

Add it.

```text
{1, 2}
```

---

### Step 3

Current element:

```text
3
```

Set:

```text
{1, 2}
```

Not found.

Add it.

```text
{1, 2, 3}
```

---

### Step 4

Current element:

```text
1
```

Set:

```text
{1, 2, 3}
```

Already exists.

Duplicate found.

Return:

```text
true
```

---

# Why Does This Work?

The set always contains all previously visited elements.

When we encounter a new element:

- If it is not in the set, we add it.
- If it is already in the set, it means we have seen it before.

Therefore, a duplicate exists.

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

Reason:

- Each element is processed once.
- Set lookup is approximately O(1).
- Set insertion is approximately O(1).

Total:

```text
O(n)
```

---

### Space Complexity

```text
O(n)
```

Reason:

In the worst case, all elements are unique and must be stored in the set.

---

# Python Solution

```python
class Solution:
    def containsDuplicate(self, nums):
        seen = set()

        for num in nums:
            if num in seen:
                return True

            seen.add(num)

        return False
```

---

# Java Solution

```java
import java.util.HashSet;

class Solution {
    public boolean containsDuplicate(int[] nums) {

        HashSet<Integer> seen = new HashSet<>();

        for (int num : nums) {

            if (seen.contains(num)) {
                return true;
            }

            seen.add(num);
        }

        return false;
    }
}
```

---

# C++ Solution

```cpp
#include <vector>
#include <unordered_set>

using namespace std;

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {

        unordered_set<int> seen;

        for (int num : nums) {

            if (seen.find(num) != seen.end()) {
                return true;
            }

            seen.insert(num);
        }

        return false;
    }
};
```

---

# Key Takeaway

Whenever a problem asks:

- Have we seen this value before?
- Is there any duplicate?
- Check uniqueness of elements

Think about using a **Set**.

A set provides fast lookup and is one of the most common tools used in coding interviews for solving duplicate-detection problems efficiently.