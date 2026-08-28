# Longest Consecutive Sequence

**LeetCode Problem:** https://leetcode.com/problems/longest-consecutive-sequence/

## Problem Statement

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

A consecutive sequence means:

```text
Each number is exactly 1 greater than the previous number.
```

The elements do not need to appear next to each other in the original array.

You must write an algorithm that runs in:

```text
O(n)
```

time. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
nums = [100,4,200,1,3,2]
```

### Output

```text
4
```

### Explanation

The longest consecutive sequence is:

```text
[1,2,3,4]
```

Length:

```text
4
```

---

## Example 2

### Input

```text
nums = [0,3,7,2,5,8,4,6,0,1]
```

### Output

```text
9
```

### Explanation

The longest consecutive sequence is:

```text
[0,1,2,3,4,5,6,7,8]
```

Length:

```text
9
```

---

# Understanding The Problem

Notice that the array is:

```text
Unsorted
```

For example:

```text
[100,4,200,1,3,2]
```

Even though:

```text
1,2,3,4
```

form a consecutive sequence, they are scattered throughout the array.

We only care about the values, not their positions.

---

# Brute Force Approach

For every number:

Keep checking:

```text
num + 1
num + 2
num + 3
...
```

until the sequence ends.

Example:

```text
Start from 1

1 exists
2 exists
3 exists
4 exists
5 doesn't exist

Length = 4
```

Repeat this for every number.

---

## Time Complexity

For each element, we may scan many other elements.

```text
O(n²)
```

Too slow.

---

# Better Approach: Sorting

## Intuition

If we sort the array:

```text
[100,4,200,1,3,2]
```

becomes:

```text
[1,2,3,4,100,200]
```

Now consecutive numbers are adjacent.

We can simply count streaks.

---

## Algorithm

Sort the array.

Traverse it.

If:

```text
current = previous + 1
```

increase streak.

Otherwise:

```text
reset streak
```

Track the maximum streak.

---

## Complexity

Sorting:

```text
O(n log n)
```

Traversal:

```text
O(n)
```

Total:

```text
O(n log n)
```

This works, but the problem specifically asks for:

```text
O(n)
```

---

# Optimal Approach: Hash Set

## Key Observation

We need:

```text
Fast lookup
```

to check whether a number exists.

A Hash Set provides:

```text
O(1)
```

average lookup time.

Therefore:

```text
Store all numbers inside a set.
```

---

# The Main Trick

Most people try to start a sequence from every number.

Example:

```text
1 -> 2 -> 3 -> 4

2 -> 3 -> 4

3 -> 4

4
```

This causes repeated work.

Instead, we only start counting from the beginning of a sequence. :contentReference[oaicite:1]{index=1}

---

# How Do We Find The Start?

A number is the start of a sequence if:

```text
num - 1 does NOT exist
```

Example:

```text
[1,2,3,4]
```

For:

```text
1
```

```text
0 does not exist
```

So:

```text
1 is the start
```

For:

```text
2
```

```text
1 exists
```

Therefore:

```text
2 is NOT the start
```

Skip it.

Similarly:

```text
3 -> Skip
4 -> Skip
```

This eliminates repeated work. :contentReference[oaicite:2]{index=2}

---

# Example Walkthrough

## Input

```text
nums = [100,4,200,1,3,2]
```

Convert to set:

```text
{
100,
4,
200,
1,
3,
2
}
```

---

### Check 100

```text
99 not present
```

Start sequence.

```text
100
```

Length:

```text
1
```

---

### Check 4

```text
3 exists
```

Not a start.

Skip.

---

### Check 200

```text
199 not present
```

Start sequence.

```text
200
```

Length:

```text
1
```

---

### Check 1

```text
0 not present
```

Start sequence.

Now keep expanding:

```text
1 exists
2 exists
3 exists
4 exists
5 does not exist
```

Length:

```text
4
```

Current answer:

```text
4
```

---

### Check 2

```text
1 exists
```

Skip.

---

### Check 3

```text
2 exists
```

Skip.

---

Final Answer:

```text
4
```

---

# Why Is It O(n)?

At first glance:

```python
while current + 1 in num_set:
```

looks like it could become:

```text
O(n²)
```

But it doesn't.

Example:

```text
1,2,3,4
```

The sequence is expanded only once:

```text
1 -> 2 -> 3 -> 4
```

After that:

```text
2
3
4
```

are skipped because they are not sequence starts.

Every number is visited at most once.

Therefore:

```text
O(n)
```

overall. :contentReference[oaicite:3]{index=3}

---

# Complexity Analysis

## Time Complexity

Building set:

```text
O(n)
```

Traversing set:

```text
O(n)
```

Total:

```text
O(n)
```

---

## Space Complexity

Hash Set:

```text
O(n)
```

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n²) | O(1) |
| Sorting | O(n log n) | O(1) |
| Hash Set (Optimal) | O(n) | O(n) |

---

# Key Takeaway

The most important insight is:

```text
Only start counting from the beginning of a sequence.
```

A number is the beginning if:

```text
num - 1 does not exist
```

This avoids repeated work and gives us the required:

```text
O(n)
```

solution. :contentReference[oaicite:4]{index=4}

---

# Python Solution

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:

        num_set = set(nums)

        longest = 0

        for num in num_set:

            if num - 1 not in num_set:

                current = num
                length = 1

                while current + 1 in num_set:
                    current += 1
                    length += 1

                longest = max(longest, length)

        return longest
```

---

# Java Solution

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int longestConsecutive(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {
            set.add(num);
        }

        int longest = 0;

        for (int num : set) {

            if (!set.contains(num - 1)) {

                int current = num;
                int length = 1;

                while (set.contains(current + 1)) {
                    current++;
                    length++;
                }

                longest = Math.max(longest, length);
            }
        }

        return longest;
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
    int longestConsecutive(vector<int>& nums) {

        unordered_set<int> numSet(nums.begin(), nums.end());

        int longest = 0;

        for (int num : numSet) {

            if (!numSet.count(num - 1)) {

                int current = num;
                int length = 1;

                while (numSet.count(current + 1)) {
                    current++;
                    length++;
                }

                longest = max(longest, length);
            }
        }

        return longest;
    }
};
```

---

## Final Takeaway

**Pattern:** Hash Set + Sequence Detection

Whenever a problem asks:

```text
Find consecutive numbers
```

or

```text
Check existence quickly
```

think about:

```text
Hash Set
```

The key trick is:

```text
Only start from numbers that do not have a predecessor.
```

For this problem:

```text
Time Complexity  : O(n)
Space Complexity : O(n)
```

which satisfies the problem's requirement.
