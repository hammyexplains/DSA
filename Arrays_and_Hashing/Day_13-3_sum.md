# 3Sum

**LeetCode Problem:** https://leetcode.com/problems/3sum/

## Problem Statement

Given an integer array `nums`, return all the triplets:

```text
[nums[i], nums[j], nums[k]]
```

such that:

```text
i != j
i != k
j != k
```

and

```text
nums[i] + nums[j] + nums[k] == 0
```

The solution set must **not contain duplicate triplets**. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
nums = [-1,0,1,2,-1,-4]
```

### Output

```text
[[-1,-1,2],[-1,0,1]]
```

### Explanation

```text
(-1) + (-1) + 2 = 0

(-1) + 0 + 1 = 0
```

These are the only unique triplets whose sum equals zero. :contentReference[oaicite:1]{index=1}

---

## Example 2

### Input

```text
nums = [0,1,1]
```

### Output

```text
[]
```

No three numbers can sum to zero. :contentReference[oaicite:2]{index=2}

---

## Example 3

### Input

```text
nums = [0,0,0]
```

### Output

```text
[[0,0,0]]
```

The only valid triplet is:

```text
0 + 0 + 0 = 0
```

:contentReference[oaicite:3]{index=3}

---

# Understanding The Problem

We need:

```text
Three numbers
```

whose sum equals:

```text
0
```

The challenge is:

```text
1. Find all valid triplets
2. Avoid duplicate triplets
```

For example:

```text
[-1,0,1,2,-1,-4]
```

contains:

```text
[-1,-1,2]
[-1,0,1]
```

but we should not return duplicates.

---

# Brute Force Approach

Try every possible triplet.

```python
for i:
    for j:
        for k:
```

Check:

```text
nums[i] + nums[j] + nums[k] == 0
```

---

## Complexity

### Time Complexity

```text
O(n³)
```

because we check every triplet.

### Space Complexity

```text
O(1)
```

This is too slow for:

```text
n = 3000
```

:contentReference[oaicite:4]{index=4}

---

# Key Insight

This problem is actually:

```text
3Sum
=
1 fixed number
+
2Sum
```

Suppose we fix:

```text
nums[i]
```

Then we need:

```text
nums[left] + nums[right]
=
-nums[i]
```

This becomes a classic:

```text
Two Sum
```

problem. :contentReference[oaicite:5]{index=5}

---

# Why Do We Sort?

Sorting provides two major advantages:

### 1. Enables Two Pointers

After sorting:

```text
[-4,-1,-1,0,1,2]
```

we can move pointers intelligently.

### 2. Makes Duplicate Handling Easy

Duplicate values become adjacent.

Example:

```text
[-4,-1,-1,0,1,2]
      ↑  ↑
```

We can easily skip duplicates. :contentReference[oaicite:6]{index=6}

---

# Optimal Approach: Sorting + Two Pointers

## Step 1

Sort the array.

Example:

```text
[-1,0,1,2,-1,-4]
```

becomes:

```text
[-4,-1,-1,0,1,2]
```

---

## Step 2

Fix one number.

Suppose:

```text
i = 1

nums[i] = -1
```

Now we need:

```text
left + right = 1
```

because:

```text
-1 + left + right = 0
```

---

## Step 3

Use Two Pointers

```text
left = i + 1
right = n - 1
```

For:

```text
[-4,-1,-1,0,1,2]
```

```text
i = 1

left = 2
right = 5
```

Values:

```text
-1 + (-1) + 2 = 0
```

Found:

```text
[-1,-1,2]
```

---

## Step 4

Move Both Pointers

After finding a triplet:

```text
left++
right--
```

Continue searching.

Next:

```text
-1 + 0 + 1 = 0
```

Found:

```text
[-1,0,1]
```

:contentReference[oaicite:7]{index=7}

---

# Two Pointer Logic

For a fixed index `i`:

```python
current_sum = nums[i] + nums[left] + nums[right]
```

### Case 1

```text
current_sum < 0
```

Need a larger value.

Move:

```python
left += 1
```

---

### Case 2

```text
current_sum > 0
```

Need a smaller value.

Move:

```python
right -= 1
```

---

### Case 3

```text
current_sum == 0
```

Valid triplet found.

Store it and move both pointers. :contentReference[oaicite:8]{index=8}

---

# Handling Duplicates

This is the most important part of the problem.

---

## Duplicate Anchor

Suppose:

```text
[-4,-1,-1,0,1,2]
```

At:

```text
i = 1 → -1
i = 2 → -1
```

If we process both, we'll generate identical triplets.

So:

```python
if i > 0 and nums[i] == nums[i - 1]:
    continue
```

Skip duplicate anchors. :contentReference[oaicite:9]{index=9}

---

## Duplicate Left Pointer

After finding a triplet:

```python
while left < right and nums[left] == nums[left + 1]:
    left += 1
```

Skip duplicate values.

---

## Duplicate Right Pointer

Similarly:

```python
while left < right and nums[right] == nums[right - 1]:
    right -= 1
```

This guarantees unique triplets. :contentReference[oaicite:10]{index=10}

---

# Optimization

Because the array is sorted:

If:

```text
nums[i] > 0
```

then:

```text
All remaining numbers are also positive.
```

Therefore:

```text
Sum can never become 0.
```

We can stop immediately.

```python
if nums[i] > 0:
    break
```

:contentReference[oaicite:11]{index=11}

---

# Dry Run

Input:

```text
[-1,0,1,2,-1,-4]
```

Sorted:

```text
[-4,-1,-1,0,1,2]
```

---

### i = 0

```text
-4
```

No valid triplet.

---

### i = 1

```text
-1
```

Pointers:

```text
left = 2
right = 5
```

```text
-1 + (-1) + 2 = 0
```

Add:

```text
[-1,-1,2]
```

---

Move pointers.

```text
-1 + 0 + 1 = 0
```

Add:

```text
[-1,0,1]
```

---

### i = 2

Duplicate:

```text
-1
```

Skip.

---

### i = 3

```text
0
```

No more valid triplets.

Final Answer:

```text
[
 [-1,-1,2],
 [-1,0,1]
]
```

:contentReference[oaicite:12]{index=12}

---

# Complexity Analysis

## Time Complexity

Sorting:

```text
O(n log n)
```

For every element, we run a two-pointer scan:

```text
O(n)
```

Overall:

```text
O(n²)
```

This is the optimal solution for 3Sum. :contentReference[oaicite:13]{index=13}

---

## Space Complexity

Ignoring output:

```text
O(1)
```

Additional memory is not required beyond pointers. :contentReference[oaicite:14]{index=14}

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n³) | O(1) |
| Sort + Two Pointers | O(n²) | O(1) |

---

# Key Takeaway

This problem teaches one of the most important interview patterns:

```text
Fix One Element
        +
Two Sum Using Two Pointers
```

The transformation is:

```text
3Sum
↓
Fix nums[i]
↓
Solve 2Sum on remaining array
```

Whenever you see:

```text
3Sum
4Sum
KSum
```

think about:

```text
Sorting
+
Two Pointers
```

---

# Python Solution

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:

        nums.sort()

        result = []

        for i in range(len(nums) - 2):

            if nums[i] > 0:
                break

            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left = i + 1
            right = len(nums) - 1

            while left < right:

                total = nums[i] + nums[left] + nums[right]

                if total < 0:
                    left += 1

                elif total > 0:
                    right -= 1

                else:

                    result.append(
                        [nums[i], nums[left], nums[right]]
                    )

                    while (
                        left < right and
                        nums[left] == nums[left + 1]
                    ):
                        left += 1

                    while (
                        left < right and
                        nums[right] == nums[right - 1]
                    ):
                        right -= 1

                    left += 1
                    right -= 1

        return result
```

---

# Java Solution

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        Arrays.sort(nums);

        List<List<Integer>> result = new ArrayList<>();

        for (int i = 0; i < nums.length - 2; i++) {

            if (nums[i] > 0)
                break;

            if (i > 0 && nums[i] == nums[i - 1])
                continue;

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {

                int sum = nums[i] +
                          nums[left] +
                          nums[right];

                if (sum < 0) {
                    left++;
                }
                else if (sum > 0) {
                    right--;
                }
                else {

                    result.add(Arrays.asList(
                        nums[i],
                        nums[left],
                        nums[right]
                    ));

                    while (
                        left < right &&
                        nums[left] == nums[left + 1]
                    ) {
                        left++;
                    }

                    while (
                        left < right &&
                        nums[right] == nums[right - 1]
                    ) {
                        right--;
                    }

                    left++;
                    right--;
                }
            }
        }

        return result;
    }
}
```

---

# C++ Solution

```cpp
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {

        sort(nums.begin(), nums.end());

        vector<vector<int>> result;

        for (int i = 0; i < nums.size() - 2; i++) {

            if (nums[i] > 0)
                break;

            if (i > 0 &&
                nums[i] == nums[i - 1])
                continue;

            int left = i + 1;
            int right = nums.size() - 1;

            while (left < right) {

                int sum =
                    nums[i] +
                    nums[left] +
                    nums[right];

                if (sum < 0) {
                    left++;
                }
                else if (sum > 0) {
                    right--;
                }
                else {

                    result.push_back({
                        nums[i],
                        nums[left],
                        nums[right]
                    });

                    while (
                        left < right &&
                        nums[left] == nums[left + 1]
                    ) {
                        left++;
                    }

                    while (
                        left < right &&
                        nums[right] == nums[right - 1]
                    ) {
                        right--;
                    }

                    left++;
                    right--;
                }
            }
        }

        return result;
    }
};
```

---

## Final Takeaway

**Pattern:** Sorting + Two Pointers

Remember:

```text
Fix One Number
↓
Convert Remaining Problem Into Two Sum
↓
Use Two Pointers
```

The most important concepts are:

```text
1. Sort the array
2. Skip duplicate anchors
3. Skip duplicate pointers
4. Use two pointers to find pairs
```

This produces the optimal:

```text
Time Complexity  : O(n²)
Space Complexity : O(1)
```

solution for the 3Sum problem.
