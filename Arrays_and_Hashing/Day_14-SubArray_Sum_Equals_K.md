# Subarray Sum Equals K

**LeetCode Problem:** https://leetcode.com/problems/subarray-sum-equals-k/

## Problem Statement

Given an integer array `nums` and an integer `k`, return the **total number of continuous subarrays** whose sum equals `k`.

A subarray is a contiguous non-empty sequence of elements within an array. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
nums = [1,1,1]
k = 2
```

### Output

```text
2
```

### Explanation

The subarrays are:

```text
[1,1]  -> indices (0,1)
[1,1]  -> indices (1,2)
```

Answer:

```text
2
```

---

## Example 2

### Input

```text
nums = [1,2,3]
k = 3
```

### Output

```text
2
```

### Explanation

The subarrays are:

```text
[1,2]
[3]
```

Answer:

```text
2
```

---

# Understanding The Problem

We need to count:

```text
How many contiguous subarrays
have sum exactly equal to k.
```

Example:

```text
nums = [1,2,3]
k = 3
```

Valid subarrays:

```text
[1,2]
[3]
```

Answer:

```text
2
```

Notice:

```text
We are counting subarrays,
not returning them.
```

---

# Brute Force Approach

Generate every possible subarray.

For every starting index:

```python
for i in range(n):
```

Keep extending the subarray:

```python
for j in range(i, n):
```

Maintain a running sum.

If:

```text
sum == k
```

increment the answer.

---

## Example

```text
nums = [1,2,3]
k = 3
```

Subarrays:

```text
[1]
[1,2]   ✓

[1,2,3]

[2]
[2,3]

[3]     ✓
```

Answer:

```text
2
```

---

## Complexity

### Time Complexity

```text
O(n²)
```

### Space Complexity

```text
O(1)
```

This works, but can be improved. :contentReference[oaicite:1]{index=1}

---

# Why Sliding Window Doesn't Work

For positive numbers only:

```text
If sum becomes too large,
we can shrink the window.
```

But here:

```text
Negative numbers are allowed.
```

Example:

```text
[3, -2, 4]
```

Removing an element may:

```text
Increase
or
Decrease
the sum
```

Therefore:

```text
Sliding Window is not reliable.
```

We need a different approach. :contentReference[oaicite:2]{index=2}

---

# Key Insight: Prefix Sum

Let's define:

```text
prefixSum[i]
=
sum of elements from index 0 to i
```

Example:

```text
nums = [1,2,3,4]
```

Prefix sums:

```text
1
3
6
10
```

---

Suppose:

```text
Current Prefix Sum = S
```

We want a subarray whose sum equals:

```text
k
```

That means:

```text
CurrentPrefix - PreviousPrefix = k
```

Rearranging:

```text
PreviousPrefix = CurrentPrefix - k
```

This is the entire trick behind the optimal solution. :contentReference[oaicite:3]{index=3}

---

# Mathematical Intuition

Suppose:

```text
nums = [1,2,3]
k = 3
```

Current prefix sum at index 2:

```text
1 + 2 + 3 = 6
```

To form a subarray with sum:

```text
3
```

we need:

```text
Previous Prefix = 6 - 3 = 3
```

Have we seen prefix sum:

```text
3
```

before?

Yes.

```text
prefix(1) = 3
```

Therefore:

```text
Subarray after that point
has sum 3
```

which is:

```text
[3]
```

---

# The HashMap Trick

Instead of storing all previous indices:

Store:

```text
prefixSum -> frequency
```

Example:

```python
{
    0 : 1,
    1 : 1,
    3 : 2,
    6 : 1
}
```

Meaning:

```text
Prefix sum 3
appeared twice.
```

Whenever we find:

```text
currentSum - k
```

inside the hashmap:

```text
Every occurrence contributes
one valid subarray.
```

:contentReference[oaicite:4]{index=4}

---

# Why Do We Start With {0:1} ?

This is one of the most important interview details.

We initialize:

```python
prefixCount = {0: 1}
```

Why?

Because before processing any element:

```text
Prefix Sum = 0
```

exists once.

This helps count subarrays that start from:

```text
index 0
```

Example:

```text
nums = [3]
k = 3
```

Current prefix:

```text
3
```

Need:

```text
3 - 3 = 0
```

Since:

```text
0 exists once
```

we correctly count:

```text
[3]
```

Without:

```python
{0:1}
```

we would miss such cases. :contentReference[oaicite:5]{index=5}

---

# Optimal Approach: Prefix Sum + HashMap

## Algorithm

Initialize:

```python
count = {0: 1}
prefix = 0
result = 0
```

For every number:

### Step 1

Update running sum:

```python
prefix += num
```

---

### Step 2

Find:

```python
needed = prefix - k
```

---

### Step 3

If:

```python
needed
```

exists in hashmap:

Add its frequency to answer.

```python
result += count[needed]
```

---

### Step 4

Store current prefix sum.

```python
count[prefix] += 1
```

Continue until the end.

---

# Dry Run

Input:

```text
nums = [1,1,1]
k = 2
```

Initialize:

```text
count = {0:1}
prefix = 0
result = 0
```

---

## First 1

```text
prefix = 1

needed = 1 - 2 = -1
```

Not found.

Store:

```text
count = {
  0:1,
  1:1
}
```

---

## Second 1

```text
prefix = 2

needed = 2 - 2 = 0
```

Found:

```text
count[0] = 1
```

Result:

```text
result = 1
```

Store:

```text
count = {
  0:1,
  1:1,
  2:1
}
```

---

## Third 1

```text
prefix = 3

needed = 3 - 2 = 1
```

Found:

```text
count[1] = 1
```

Result:

```text
result = 2
```

Final Answer:

```text
2
```

:contentReference[oaicite:6]{index=6}

---

# Why This Works

We know:

```text
Subarray Sum
=
Current Prefix
-
Previous Prefix
```

Whenever:

```text
Previous Prefix
=
Current Prefix - k
```

a valid subarray exists.

The hashmap instantly tells us:

```text
How many such prefix sums
have appeared before.
```

Thus every valid subarray gets counted exactly once. :contentReference[oaicite:7]{index=7}

---

# Complexity Analysis

## Time Complexity

Single traversal:

```text
O(n)
```

HashMap operations:

```text
O(1)
```

Average time.

Overall:

```text
O(n)
```

---

## Space Complexity

HashMap may store:

```text
n prefix sums
```

Therefore:

```text
O(n)
```

:contentReference[oaicite:8]{index=8}

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n²) | O(1) |
| Prefix Sum + HashMap | O(n) | O(n) |

---

# Key Takeaway

This problem teaches one of the most important interview patterns:

```text
Prefix Sum + HashMap
```

Remember the equation:

```text
Current Prefix - Previous Prefix = k
```

Rearrange:

```text
Previous Prefix = Current Prefix - k
```

Whenever you see:

```text
Count subarrays
with a given sum
```

think about:

```text
Prefix Sum
+
HashMap
```

---

# Python Solution

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:

        prefix_count = {0: 1}

        prefix_sum = 0
        result = 0

        for num in nums:

            prefix_sum += num

            result += prefix_count.get(
                prefix_sum - k,
                0
            )

            prefix_count[prefix_sum] = (
                prefix_count.get(prefix_sum, 0) + 1
            )

        return result
```

---

# Java Solution

```java
import java.util.HashMap;

class Solution {
    public int subarraySum(int[] nums, int k) {

        HashMap<Integer, Integer> prefixCount =
            new HashMap<>();

        prefixCount.put(0, 1);

        int prefixSum = 0;
        int result = 0;

        for (int num : nums) {

            prefixSum += num;

            result += prefixCount.getOrDefault(
                prefixSum - k,
                0
            );

            prefixCount.put(
                prefixSum,
                prefixCount.getOrDefault(
                    prefixSum,
                    0
                ) + 1
            );
        }

        return result;
    }
}
```

---

# C++ Solution

```cpp
#include <vector>
#include <unordered_map>

using namespace std;

class Solution {
public:
    int subarraySum(
        vector<int>& nums,
        int k
    ) {

        unordered_map<int, int> prefixCount;

        prefixCount[0] = 1;

        int prefixSum = 0;
        int result = 0;

        for (int num : nums) {

            prefixSum += num;

            result += prefixCount[
                prefixSum - k
            ];

            prefixCount[prefixSum]++;
        }

        return result;
    }
};
```

---

## Final Takeaway

**Pattern:** Prefix Sum + HashMap

Whenever you need to:

```text
Count subarrays
with a target sum
```

remember:

```text
Current Prefix - Previous Prefix = Target
```

Store previous prefix sums in a hashmap and use:

```text
prefixSum - k
```

as the lookup value.

For this problem:

```text
Time Complexity  : O(n)
Space Complexity : O(n)
```

which is the optimal solution.
