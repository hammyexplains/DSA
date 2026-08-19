# Two Sum

**LeetCode Problem:** https://leetcode.com/problems/two-sum/

## Problem Statement

Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to the target.

You may assume that:

- Each input has exactly one solution.
- You may not use the same element twice.
- You can return the answer in any order.

---

## Example 1

**Input**

```text
nums = [2,7,11,15]
target = 9
```

**Output**

```text
[0,1]
```

**Explanation**

```text
nums[0] + nums[1]
= 2 + 7
= 9
```

Therefore, the answer is:

```text
[0,1]
```

---

## Example 2

**Input**

```text
nums = [3,2,4]
target = 6
```

**Output**

```text
[1,2]
```

---

## Example 3

**Input**

```text
nums = [3,3]
target = 6
```

**Output**

```text
[0,1]
```

---

# Understanding the Problem

We need to find two numbers whose sum equals the given target.

Instead of returning the numbers themselves, we must return their indices.

For every number:

```text
current_number
```

we need another number:

```text
target - current_number
```

This value is called the **complement**.

---

# Brute Force Approach

## Idea

Check every possible pair.

### Algorithm

1. Pick the first element.
2. Compare it with all remaining elements.
3. If their sum equals the target, return the indices.
4. Otherwise continue checking.

### Example

```text
nums = [2,7,11,15]
target = 9
```

Check:

```text
2 + 7 = 9
```

Target found.

Return:

```text
[0,1]
```

### Time Complexity

```text
O(n²)
```

### Space Complexity

```text
O(1)
```

### Why is it inefficient?

For every element, we scan the remaining elements again.

As the input size grows, the number of comparisons increases rapidly.

---

# Optimized Approach Using HashMap

## Intuition

Instead of searching the entire array for the complement every time, we can store previously seen numbers in a HashMap.

For every element:

```text
complement = target - current_number
```

If the complement already exists in the HashMap, we have found the answer.

Otherwise, store the current number and its index.

---

# Algorithm

1. Create an empty HashMap.
2. Traverse the array.
3. Calculate:

```text
complement = target - current_number
```

4. Check if the complement already exists in the HashMap.
5. If yes, return both indices.
6. Otherwise, store the current number and its index.
7. Continue until a solution is found.

---

# Dry Run

### Input

```text
nums = [2,7,11,15]
target = 9
```

### Initial HashMap

```text
{}
```

---

### Iteration 1

Current number:

```text
2
```

Complement:

```text
9 - 2 = 7
```

HashMap:

```text
{}
```

Not found.

Store:

```text
{
  2 : 0
}
```

---

### Iteration 2

Current number:

```text
7
```

Complement:

```text
9 - 7 = 2
```

HashMap:

```text
{
  2 : 0
}
```

Complement found.

Return:

```text
[0,1]
```

---

# Why Does This Work?

The HashMap stores all previously visited numbers.

When processing a new number:

```text
current_number
```

we immediately know whether the required complement has already appeared.

This avoids scanning the array repeatedly and reduces the complexity from:

```text
O(n²)
```

to:

```text
O(n)
```

---

# Complexity Analysis

## Time Complexity

```text
O(n)
```

Reason:

- Each element is processed once.
- HashMap lookup is O(1) on average.
- HashMap insertion is O(1) on average.

Total:

```text
O(n)
```

---

## Space Complexity

```text
O(n)
```

Reason:

In the worst case, every element is stored in the HashMap.

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n²) | O(1) |
| HashMap | O(n) | O(n) |

---

# Key Insight

For every element:

```text
current_number
```

we do not need to search the entire array.

We only need to know:

```text
Have I already seen the complement?
```

This is a classic **HashMap lookup pattern** used in many interview problems.

---

# Python Solution

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:

        seen = {}

        for i, num in enumerate(nums):

            complement = target - num

            if complement in seen:
                return [seen[complement], i]

            seen[num] = i
```

---

# Java Solution

```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> seen = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int complement = target - nums[i];

            if (seen.containsKey(complement)) {
                return new int[]{seen.get(complement), i};
            }

            seen.put(nums[i], i);
        }

        return new int[]{};
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
    vector<int> twoSum(vector<int>& nums, int target) {

        unordered_map<int, int> seen;

        for (int i = 0; i < nums.size(); i++) {

            int complement = target - nums[i];

            if (seen.find(complement) != seen.end()) {
                return {seen[complement], i};
            }

            seen[nums[i]] = i;
        }

        return {};
    }
};
```

---

# Key Takeaway

Whenever a problem asks:

- Find a pair with a specific sum
- Check whether a complement exists
- Search for previously seen values

Think about using a **HashMap**.

The HashMap lookup pattern is one of the most important patterns in coding interviews and appears frequently in array, string, and hashing problems.
