# Contains Duplicate II

**LeetCode Problem:** https://leetcode.com/problems/contains-duplicate-ii/

## Problem Statement

Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the array such that:

```text
nums[i] == nums[j]
```

and

```text
abs(i - j) <= k
```

Otherwise, return `false`. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
nums = [1,2,3,1]
k = 3
```

### Output

```text
true
```

### Explanation

```text
nums[0] = 1
nums[3] = 1

|0 - 3| = 3
```

Since:

```text
3 <= k
```

the answer is:

```text
true
```

---

## Example 2

### Input

```text
nums = [1,0,1,1]
k = 1
```

### Output

```text
true
```

### Explanation

```text
nums[2] = 1
nums[3] = 1

|2 - 3| = 1
```

Therefore:

```text
true
```

---

## Example 3

### Input

```text
nums = [1,2,3,1,2,3]
k = 2
```

### Output

```text
false
```

### Explanation

Although duplicates exist, none of them appear within a distance of `2`. :contentReference[oaicite:1]{index=1}

---

# Understanding the Problem

This is an extension of:

```text
Contains Duplicate
```

In the original problem, we only checked:

```text
Does a duplicate exist?
```

In this problem, we additionally need to check:

```text
Are the duplicate indices close enough?
```

Specifically:

```text
distance <= k
```

---

# Brute Force Approach

For every element:

Check all elements within distance `k`.

```python
for i in range(len(nums)):
    for j in range(i + 1, len(nums)):
        if nums[i] == nums[j]:
            if abs(i - j) <= k:
                return True
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

This is too slow for large inputs.

---

# Optimal Approach 1: HashMap (Most Common Solution)

## Key Observation

Suppose we encounter:

```text
nums = [1,2,3,1]
```

When we reach the second `1`, we only care about:

```text
Where did we most recently see 1?
```

We do not need all previous indices.

We only need the latest index.

Therefore we store:

```text
number -> latest index
```

inside a HashMap. :contentReference[oaicite:2]{index=2}

---

# Example Walkthrough

### Input

```text
nums = [1,2,3,1]
k = 3
```

---

### Index 0

```text
num = 1
```

Store:

```text
{
    1 : 0
}
```

---

### Index 1

```text
num = 2
```

Store:

```text
{
    1 : 0,
    2 : 1
}
```

---

### Index 2

```text
num = 3
```

Store:

```text
{
    1 : 0,
    2 : 1,
    3 : 2
}
```

---

### Index 3

```text
num = 1
```

Already exists.

Previous index:

```text
0
```

Distance:

```text
3 - 0 = 3
```

Since:

```text
3 <= k
```

Return:

```text
true
```

---

# Algorithm

Traverse the array.

For every element:

### Step 1

Check if it already exists in the HashMap.

### Step 2

If yes:

```text
current_index - previous_index <= k
```

Return:

```text
True
```

### Step 3

Update its latest index.

### Step 4

If traversal completes:

```text
Return False
```

---

# Complexity Analysis

### Time Complexity

Each element is processed once.

```text
O(n)
```

### Space Complexity

HashMap stores at most all unique elements.

```text
O(n)
```

:contentReference[oaicite:3]{index=3}

---

# Approach 2: Sliding Window + HashSet

## Key Observation

We only care about elements within distance:

```text
k
```

So we can maintain a sliding window of size `k`.

If a duplicate exists inside the window:

```text
Return True
```

Otherwise continue. :contentReference[oaicite:4]{index=4}

---

# Example

### Input

```text
nums = [1,2,3,1]
k = 3
```

Window:

```text
{1}
```

Add:

```text
{1,2}
```

Add:

```text
{1,2,3}
```

Next element:

```text
1
```

Already inside window.

Return:

```text
true
```

---

# Why Sliding Window Works

If two equal numbers are more than `k` positions apart:

```text
They cannot satisfy the condition.
```

Therefore, we only need to keep track of the most recent `k` elements.

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

### Space Complexity

Window size is at most:

```text
k
```

Therefore:

```text
O(min(n, k))
```

:contentReference[oaicite:5]{index=5}

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n²) | O(1) |
| HashMap | O(n) | O(n) |
| Sliding Window + Set | O(n) | O(min(n,k)) |

---

# Key Takeaway

This problem teaches an important pattern:

```text
Value → Most Recent Index
```

Whenever a problem asks:

```text
Have I seen this value before?
```

and

```text
How far away was it?
```

think about:

```text
HashMap
```

where:

```text
key   = value
value = latest index
```

---

# Python Solutions

## Approach 1: HashMap

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:

        seen = {}

        for i, num in enumerate(nums):

            if num in seen and i - seen[num] <= k:
                return True

            seen[num] = i

        return False
```

---

## Approach 2: Sliding Window + Set

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:

        window = set()

        for i, num in enumerate(nums):

            if i > k:
                window.remove(nums[i - k - 1])

            if num in window:
                return True

            window.add(num)

        return False
```

---

# Java Solutions

## HashMap

```java
import java.util.HashMap;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            if (map.containsKey(nums[i]) &&
                i - map.get(nums[i]) <= k) {
                return true;
            }

            map.put(nums[i], i);
        }

        return false;
    }
}
```

---

## Sliding Window + HashSet

```java
import java.util.HashSet;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {

        HashSet<Integer> window = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {

            if (i > k) {
                window.remove(nums[i - k - 1]);
            }

            if (window.contains(nums[i])) {
                return true;
            }

            window.add(nums[i]);
        }

        return false;
    }
}
```

---

# C++ Solutions

## HashMap

```cpp
#include <vector>
#include <unordered_map>

using namespace std;

class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {

        unordered_map<int, int> seen;

        for (int i = 0; i < nums.size(); i++) {

            if (seen.count(nums[i]) &&
                i - seen[nums[i]] <= k) {
                return true;
            }

            seen[nums[i]] = i;
        }

        return false;
    }
};
```

---

## Sliding Window + HashSet

```cpp
#include <vector>
#include <unordered_set>

using namespace std;

class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {

        unordered_set<int> window;

        for (int i = 0; i < nums.size(); i++) {

            if (i > k) {
                window.erase(nums[i - k - 1]);
            }

            if (window.count(nums[i])) {
                return true;
            }

            window.insert(nums[i]);
        }

        return false;
    }
};
```

---

## Final Takeaway

**Pattern:** HashMap + Sliding Window

The most common interview solution is:

```text
Value → Latest Index
```

using a HashMap.

Whenever you need to track:

```text
Duplicate values
+
Distance between occurrences
```

think about:

```text
HashMap storing indices
```

For this problem:

```text
Time Complexity  : O(n)
Space Complexity : O(n)
```

which is the optimal solution.
