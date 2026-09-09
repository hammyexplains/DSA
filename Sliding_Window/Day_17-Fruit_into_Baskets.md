# Fruit Into Baskets

**LeetCode Problem:** https://leetcode.com/problems/fruit-into-baskets/

## Problem Statement

You are given an integer array `fruits` where:

```text
fruits[i]
```

represents the type of fruit produced by the `i-th` tree.

You have:

```text
2 baskets
```

and each basket can hold only **one type of fruit**, but an unlimited quantity of that type.

Starting from any tree, you must pick exactly one fruit from every tree while moving to the right.

You must stop when you encounter a fruit that cannot fit into either basket.

Return:

```text
The maximum number of fruits you can collect.
```

([leetcode.com](https://leetcode.com/problems/fruit-into-baskets/))

---

## Example 1

### Input

```text
fruits = [1,2,1]
```

### Output

```text
3
```

### Explanation

We can collect:

```text
[1,2,1]
```

Only two fruit types exist.

Total:

```text
3
```

---

## Example 2

### Input

```text
fruits = [0,1,2,2]
```

### Output

```text
3
```

### Explanation

Collect:

```text
[1,2,2]
```

Total:

```text
3
```

---

## Example 3

### Input

```text
fruits = [1,2,3,2,2]
```

### Output

```text
4
```

### Explanation

Collect:

```text
[2,3,2,2]
```

Total:

```text
4
```

---

# Understanding The Problem

We need the:

```text
Longest contiguous subarray
```

containing at most:

```text
2 distinct numbers
```

Example:

```text
[1,2,3,2,2]
```

Valid windows:

```text
[1,2]
[2,3]
[3,2,2]
[2,3,2,2]
```

The longest one is:

```text
[2,3,2,2]
```

Length:

```text
4
```

---

# Reframing The Problem

Instead of thinking about baskets:

Think about:

```text
Find the longest subarray
with at most 2 distinct values.
```

This transformation makes the solution much easier.

([neetcode.io](https://neetcode.io/solutions/fruit-into-baskets))

---

# Brute Force Approach

For every starting position:

```python
for i in range(n):
```

Expand to the right:

```python
for j in range(i, n):
```

Count distinct fruit types.

If distinct fruits become:

```text
> 2
```

stop.

Track the maximum length.

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

Too slow for large inputs.

---

# Key Insight

We need:

```text
Longest valid subarray
```

This is a classic sign of:

```text
Sliding Window
```

The window should always contain:

```text
At most 2 fruit types.
```

Whenever we exceed:

```text
2 distinct fruits
```

we shrink the window.

([algo.monster](https://algo.monster/liteproblems/904))

---

# Optimal Approach: Sliding Window + HashMap

We'll maintain:

```text
left pointer
right pointer
```

and a hashmap:

```python
fruit_count
```

that stores:

```text
fruit -> frequency
```

inside the current window.

---

# Window Rule

The window is valid if:

```text
Number of distinct fruits <= 2
```

As soon as:

```text
Number of distinct fruits > 2
```

we move the left pointer until the window becomes valid again.

---

# Visual Example

Input:

```text
[1,2,3,2,2]
```

---

### Expand Window

```text
[1]
```

Distinct:

```text
1
```

Valid.

---

```text
[1,2]
```

Distinct:

```text
2
```

Valid.

---

```text
[1,2,3]
```

Distinct:

```text
3
```

Invalid.

Shrink from left.

---

Remove:

```text
1
```

Window becomes:

```text
[2,3]
```

Distinct:

```text
2
```

Valid again.

---

Expand further:

```text
[2,3,2]
```

Valid.

---

```text
[2,3,2,2]
```

Valid.

Length:

```text
4
```

Maximum answer.

---

# Why HashMap?

The hashmap stores:

```python
{
    fruit_type : count
}
```

Example:

```python
{
    2 : 3,
    3 : 1
}
```

Meaning:

```text
Fruit 2 appears 3 times
Fruit 3 appears 1 time
```

inside the current window.

When a count becomes:

```text
0
```

we remove that fruit type from the hashmap.

([algo.monster](https://algo.monster/liteproblems/904))

---

# Algorithm

Initialize:

```python
left = 0
max_fruits = 0
fruit_count = {}
```

---

For each:

```python
right
```

Add the current fruit.

```python
fruit_count[fruit] += 1
```

---

If distinct fruits become:

```text
> 2
```

Shrink window:

```python
while len(fruit_count) > 2:
```

Remove fruits from the left.

If a frequency becomes:

```text
0
```

delete it.

---

After every valid window:

```python
max_fruits = max(
    max_fruits,
    right - left + 1
)
```

---

# Dry Run

Input:

```text
[1,2,3,2,2]
```

---

### right = 0

Window:

```text
[1]
```

Length:

```text
1
```

Answer:

```text
1
```

---

### right = 1

Window:

```text
[1,2]
```

Length:

```text
2
```

Answer:

```text
2
```

---

### right = 2

Window:

```text
[1,2,3]
```

Distinct:

```text
3
```

Invalid.

Shrink.

Window:

```text
[2,3]
```

Length:

```text
2
```

---

### right = 3

Window:

```text
[2,3,2]
```

Length:

```text
3
```

Answer:

```text
3
```

---

### right = 4

Window:

```text
[2,3,2,2]
```

Length:

```text
4
```

Answer:

```text
4
```

Final answer:

```text
4
```

---

# Why This Works

The window always satisfies:

```text
At most 2 fruit types.
```

Whenever it becomes invalid:

```text
Shrink from the left.
```

Because every element enters and leaves the window at most once:

```text
O(n)
```

time is achieved.

([neetcode.io](https://neetcode.io/solutions/fruit-into-baskets))

---

# Complexity Analysis

Let:

```text
n = length of fruits
```

### Time Complexity

Each element:

```text
Added once
Removed once
```

Therefore:

```text
O(n)
```

---

### Space Complexity

At most:

```text
3 fruit types
```

can temporarily exist before shrinking.

Therefore:

```text
O(1)
```

More formally:

```text
O(3) ≈ O(1)
```

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n²) | O(1) |
| Sliding Window + HashMap | O(n) | O(1) |

---

# Pattern Recognition

When you see:

```text
Longest Subarray
At Most K Distinct Elements
```

think:

```text
Sliding Window
+
HashMap
```

This exact pattern appears in:

- Fruit Into Baskets
- Longest Substring with At Most K Distinct Characters
- Longest Repeating Character Replacement
- Subarrays with K Distinct Integers

---

# Python Solution

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:

        fruit_count = {}

        left = 0
        max_fruits = 0

        for right in range(len(fruits)):

            fruit = fruits[right]

            fruit_count[fruit] = (
                fruit_count.get(fruit, 0) + 1
            )

            while len(fruit_count) > 2:

                left_fruit = fruits[left]

                fruit_count[left_fruit] -= 1

                if fruit_count[left_fruit] == 0:
                    del fruit_count[left_fruit]

                left += 1

            max_fruits = max(
                max_fruits,
                right - left + 1
            )

        return max_fruits
```

---

# Java Solution

```java
import java.util.*;

class Solution {
    public int totalFruit(int[] fruits) {

        Map<Integer, Integer> fruitCount =
            new HashMap<>();

        int left = 0;
        int maxFruits = 0;

        for (int right = 0;
             right < fruits.length;
             right++) {

            fruitCount.put(
                fruits[right],
                fruitCount.getOrDefault(
                    fruits[right],
                    0
                ) + 1
            );

            while (fruitCount.size() > 2) {

                fruitCount.put(
                    fruits[left],
                    fruitCount.get(
                        fruits[left]
                    ) - 1
                );

                if (
                    fruitCount.get(
                        fruits[left]
                    ) == 0
                ) {
                    fruitCount.remove(
                        fruits[left]
                    );
                }

                left++;
            }

            maxFruits = Math.max(
                maxFruits,
                right - left + 1
            );
        }

        return maxFruits;
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
    int totalFruit(
        vector<int>& fruits
    ) {

        unordered_map<int, int> fruitCount;

        int left = 0;
        int maxFruits = 0;

        for (
            int right = 0;
            right < fruits.size();
            right++
        ) {

            fruitCount[
                fruits[right]
            ]++;

            while (
                fruitCount.size() > 2
            ) {

                fruitCount[
                    fruits[left]
                ]--;

                if (
                    fruitCount[
                        fruits[left]
                    ] == 0
                ) {
                    fruitCount.erase(
                        fruits[left]
                    );
                }

                left++;
            }

            maxFruits = max(
                maxFruits,
                right - left + 1
            );
        }

        return maxFruits;
    }
};
```

---

## Final Takeaway

**Pattern:** Sliding Window + HashMap

Remember:

```text
2 Baskets
```

really means:

```text
At Most 2 Distinct Fruit Types
```

Maintain:

```text
A valid window
```

using a hashmap.

Whenever distinct fruits exceed:

```text
2
```

shrink the window until it becomes valid again.

Result:

```text
Time Complexity  : O(n)
Space Complexity : O(1)
```

which is the optimal solution for **Fruit Into Baskets**.
