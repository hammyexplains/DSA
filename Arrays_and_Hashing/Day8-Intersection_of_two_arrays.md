# Intersection of Two Arrays

**LeetCode Problem:** https://leetcode.com/problems/intersection-of-two-arrays/

## Problem Statement

Given two integer arrays `nums1` and `nums2`, return an array containing their intersection.

### Rules

- Each element in the result must be unique.
- The result can be returned in any order. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
nums1 = [1,2,2,1]
nums2 = [2,2]
```

### Output

```text
[2]
```

---

## Example 2

### Input

```text
nums1 = [4,9,5]
nums2 = [9,4,9,8,4]
```

### Output

```text
[4,9]
```

or

```text
[9,4]
```

Both are correct because order does not matter. :contentReference[oaicite:1]{index=1}

---

# Understanding the Problem

We need to find numbers that exist in both arrays.

Example:

```text
nums1 = [1,2,2,1]
nums2 = [2,2]
```

Common numbers:

```text
2
```

Even though `2` appears multiple times, we only return it once because the result must contain unique elements.

---

# Approach 1: Hash Set (Optimal)

## Intuition

Sets automatically remove duplicates and provide:

```text
O(1)
```

average lookup time.

So:

1. Convert `nums1` into a set.
2. Convert `nums2` into a set.
3. Find the common elements.
4. Return the result.

This is the cleanest and most common interview solution. :contentReference[oaicite:2]{index=2}

---

## Example

### Input

```text
nums1 = [1,2,2,1]
nums2 = [2,2]
```

Convert to sets:

```text
set1 = {1,2}
set2 = {2}
```

Intersection:

```text
set1 ∩ set2
```

Result:

```text
{2}
```

Convert back to list:

```text
[2]
```

---

## Algorithm

### Step 1

Convert arrays into sets.

```python
set1 = set(nums1)
set2 = set(nums2)
```

### Step 2

Find intersection.

```python
set1 & set2
```

### Step 3

Convert to list.

```python
return list(set1 & set2)
```

---

## Complexity Analysis

Let:

- `n = len(nums1)`
- `m = len(nums2)`

### Time Complexity

Creating first set:

```text
O(n)
```

Creating second set:

```text
O(m)
```

Intersection:

```text
O(min(n,m))
```

Overall:

```text
O(n + m)
```

### Space Complexity

Two sets:

```text
O(n + m)
```

:contentReference[oaicite:3]{index=3}

---

# Approach 2: Hash Set + Iteration

Instead of using the built-in intersection operator, we can manually check membership.

---

## Intuition

Store all elements of one array inside a set.

Then iterate through the other array and collect common elements.

Use another set to avoid duplicates.

---

## Example

```text
nums1 = [4,9,5]
nums2 = [9,4,9,8,4]
```

Store:

```text
set1 = {4,9,5}
```

Traverse nums2:

```text
9 → found
4 → found
9 → already added
8 → not found
4 → already added
```

Result:

```text
[9,4]
```

---

## Complexity Analysis

### Time Complexity

```text
O(n + m)
```

### Space Complexity

```text
O(n)
```

:contentReference[oaicite:4]{index=4}

---

# Approach 3: Sorting + Two Pointers

## Intuition

If both arrays are sorted:

```text
nums1 = [1,2,2,4]
nums2 = [2,2,3,4]
```

We can use two pointers to find common elements efficiently.

---

## Example

```text
nums1 = [1,2,2,4]
nums2 = [2,2,3,4]
```

```text
i = 0
j = 0
```

Compare:

```text
1 < 2
```

Move `i`.

Compare:

```text
2 == 2
```

Add `2`.

Continue until both arrays are traversed.

---

## Complexity Analysis

Sorting:

```text
O(n log n + m log m)
```

Traversal:

```text
O(n + m)
```

Overall:

```text
O(n log n + m log m)
```

Space:

```text
O(1)
```

(excluding output)

:contentReference[oaicite:5]{index=5}

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Hash Set Intersection | O(n + m) | O(n + m) |
| Hash Set + Iteration | O(n + m) | O(n) |
| Sorting + Two Pointers | O(n log n + m log m) | O(1) |

---

# Key Takeaway

This problem is fundamentally a:

```text
Hash Set Problem
```

The important observations are:

1. Result must contain unique elements.
2. Order does not matter.
3. Fast membership checking is required.

Whenever you see:

```text
Find common unique elements
```

think about:

```text
Hash Set
```

because sets naturally handle:

```text
Uniqueness + O(1) Lookup
```

---

# Python Solutions

## Approach 1: Built-in Set Intersection

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```

---

## Approach 2: Manual Hash Set

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

        seen = set(nums1)
        result = set()

        for num in nums2:
            if num in seen:
                result.add(num)

        return list(result)
```

---

## Approach 3: Sorting + Two Pointers

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

        nums1.sort()
        nums2.sort()

        i = 0
        j = 0

        result = set()

        while i < len(nums1) and j < len(nums2):

            if nums1[i] == nums2[j]:
                result.add(nums1[i])
                i += 1
                j += 1

            elif nums1[i] < nums2[j]:
                i += 1

            else:
                j += 1

        return list(result)
```

---

# Java Solutions

## Approach 1: Hash Set

```java
import java.util.*;

class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {

        Set<Integer> set1 = new HashSet<>();
        Set<Integer> result = new HashSet<>();

        for (int num : nums1) {
            set1.add(num);
        }

        for (int num : nums2) {
            if (set1.contains(num)) {
                result.add(num);
            }
        }

        return result.stream()
                     .mapToInt(Integer::intValue)
                     .toArray();
    }
}
```

---

# C++ Solutions

## Approach 1: Hash Set

```cpp
#include <vector>
#include <unordered_set>

using namespace std;

class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {

        unordered_set<int> set1(nums1.begin(), nums1.end());
        unordered_set<int> result;

        for (int num : nums2) {
            if (set1.count(num)) {
                result.insert(num);
            }
        }

        return vector<int>(result.begin(), result.end());
    }
};
```

---

## Final Takeaway

**Pattern:** Hash Set

Whenever a problem asks:

```text
Find common unique elements
```

or

```text
Check membership efficiently
```

think about:

```text
Hash Set
```

For this problem, the optimal solution is:

```text
Time Complexity  : O(n + m)
Space Complexity : O(n + m)
```

using set intersection.
