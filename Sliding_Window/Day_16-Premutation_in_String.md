# Permutation in String

**LeetCode Problem:** https://leetcode.com/problems/permutation-in-string/

## Problem Statement

Given two strings:

```text
s1
s2
```

Return:

```text
true
```

if `s2` contains a permutation of `s1`, otherwise return:

```text
false
```

In other words, determine whether any substring of `s2` is an anagram of `s1`. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
s1 = "ab"
s2 = "eidbaooo"
```

### Output

```text
true
```

### Explanation

The substring:

```text
"ba"
```

is a permutation of:

```text
"ab"
```

Therefore:

```text
true
```

:contentReference[oaicite:1]{index=1}

---

## Example 2

### Input

```text
s1 = "ab"
s2 = "eidboaoo"
```

### Output

```text
false
```

### Explanation

No substring of length:

```text
2
```

contains the same character frequencies as:

```text
"ab"
```

:contentReference[oaicite:2]{index=2}

---

# Understanding The Problem

A permutation means:

```text
Same characters
Same frequencies
Different order allowed
```

Example:

```text
"abc"
```

Permutations:

```text
abc
acb
bac
bca
cab
cba
```

All have:

```text
a = 1
b = 1
c = 1
```

Therefore:

```text
We do not care about order.
We only care about character frequencies.
```

:contentReference[oaicite:3]{index=3}

---

# Key Observation

If:

```text
len(s1) = 3
```

then any valid permutation inside `s2` must also have length:

```text
3
```

Therefore:

```text
We only need to examine
windows of size len(s1)
```

inside `s2`.

This immediately suggests:

```text
Sliding Window
```

:contentReference[oaicite:4]{index=4}

---

# Brute Force Approach

Generate every substring of length:

```text
len(s1)
```

For each substring:

1. Sort substring
2. Sort s1
3. Compare

Example:

```python
sorted(substring) == sorted(s1)
```

---

## Complexity

If:

```text
m = len(s1)
n = len(s2)
```

Then:

### Time Complexity

```text
O((n - m + 1) × m log m)
```

### Space Complexity

```text
O(m)
```

Too slow for large inputs. :contentReference[oaicite:5]{index=5}

---

# Optimal Approach: Sliding Window + Frequency Count

## Main Idea

A permutation has:

```text
Same character frequencies
```

Instead of sorting every window:

```text
Maintain character counts
```

for:

```text
s1
current window in s2
```

If both frequency arrays become equal:

```text
Permutation found
```

:contentReference[oaicite:6]{index=6}

---

# Why Frequency Arrays?

The problem states:

```text
Only lowercase English letters
```

which means:

```text
26 possible characters
```

We can use:

```python
count = [0] * 26
```

where:

```text
index 0 -> a
index 1 -> b
...
index 25 -> z
```

This gives:

```text
O(1)
```

access time. :contentReference[oaicite:7]{index=7}

---

# Sliding Window Intuition

Suppose:

```text
s1 = "ab"
s2 = "eidbaooo"
```

Window size:

```text
2
```

Windows:

```text
ei
id
db
ba
ao
oo
oo
```

We compare each window's frequency count with:

```text
ab
```

When we reach:

```text
ba
```

Frequencies match.

Return:

```text
true
```

:contentReference[oaicite:8]{index=8}

---

# Efficient Window Updates

Instead of recalculating counts every time:

When window slides:

### Add

New character entering from the right.

```python
window[right] += 1
```

### Remove

Old character leaving from the left.

```python
window[left] -= 1
```

Thus each move costs:

```text
O(1)
```

:contentReference[oaicite:9]{index=9}

---

# Dry Run

## Input

```text
s1 = "ab"
s2 = "eidbaooo"
```

---

### Target Count

```text
a = 1
b = 1
```

---

### First Window

```text
"ei"
```

Not equal.

---

### Slide

```text
"id"
```

Not equal.

---

### Slide

```text
"db"
```

Not equal.

---

### Slide

```text
"ba"
```

Counts become:

```text
a = 1
b = 1
```

Exactly matches:

```text
s1
```

Return:

```text
true
```

:contentReference[oaicite:10]{index=10}

---

# Even Better Optimization: Matches Counter

Comparing two arrays of size:

```text
26
```

is already very fast.

However, we can optimize further.

Maintain:

```text
matches
```

which stores:

```text
How many character positions currently match.
```

Example:

```text
a counts match
b counts match
c counts match
...
```

If:

```text
matches == 26
```

then:

```text
All frequencies match
```

and a permutation exists. :contentReference[oaicite:11]{index=11}

---

# Algorithm

### Step 1

If:

```python
len(s1) > len(s2)
```

return:

```python
False
```

---

### Step 2

Build frequency arrays:

```python
s1Count
windowCount
```

---

### Step 3

Fill first window of size:

```python
len(s1)
```

---

### Step 4

Compare counts.

If equal:

```python
return True
```

---

### Step 5

Slide window:

```python
Add right character
Remove left character
```

---

### Step 6

After every slide:

```python
Compare counts
```

If equal:

```python
return True
```

---

### Step 7

If traversal finishes:

```python
return False
```

:contentReference[oaicite:12]{index=12}

---

# Complexity Analysis

Let:

```text
m = len(s1)
n = len(s2)
```

### Time Complexity

Building counts:

```text
O(m)
```

Sliding window:

```text
O(n)
```

Comparisons:

```text
26 = constant
```

Overall:

```text
O(n)
```

### Space Complexity

Two arrays of size:

```text
26
```

Therefore:

```text
O(1)
```

extra space. :contentReference[oaicite:13]{index=13}

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Sort Every Window | O((n-m+1) × m log m) | O(m) |
| Sliding Window + Frequency Array | O(n) | O(1) |
| Sliding Window + Matches Counter | O(n) | O(1) |

---

# Key Takeaway

This problem teaches one of the most important sliding window patterns:

```text
Fixed Size Window
+
Frequency Counting
```

Whenever you hear:

```text
Permutation
Anagram
Same character frequencies
```

think:

```text
Sliding Window
+
Character Count Array
```

The critical observation is:

```text
A permutation must have
the same length and
the same character frequencies.
```

---

# Python Solution

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:

        if len(s1) > len(s2):
            return False

        s1Count = [0] * 26
        windowCount = [0] * 26

        for i in range(len(s1)):
            s1Count[ord(s1[i]) - ord('a')] += 1
            windowCount[ord(s2[i]) - ord('a')] += 1

        if s1Count == windowCount:
            return True

        left = 0

        for right in range(len(s1), len(s2)):

            windowCount[
                ord(s2[right]) - ord('a')
            ] += 1

            windowCount[
                ord(s2[left]) - ord('a')
            ] -= 1

            left += 1

            if s1Count == windowCount:
                return True

        return False
```

---

# Java Solution

```java
class Solution {
    public boolean checkInclusion(
        String s1,
        String s2
    ) {

        if (s1.length() > s2.length()) {
            return false;
        }

        int[] s1Count = new int[26];
        int[] windowCount = new int[26];

        for (int i = 0; i < s1.length(); i++) {
            s1Count[s1.charAt(i) - 'a']++;
            windowCount[s2.charAt(i) - 'a']++;
        }

        if (java.util.Arrays.equals(
                s1Count,
                windowCount
        )) {
            return true;
        }

        int left = 0;

        for (
            int right = s1.length();
            right < s2.length();
            right++
        ) {

            windowCount[
                s2.charAt(right) - 'a'
            ]++;

            windowCount[
                s2.charAt(left) - 'a'
            ]--;

            left++;

            if (java.util.Arrays.equals(
                    s1Count,
                    windowCount
            )) {
                return true;
            }
        }

        return false;
    }
}
```

---

# C++ Solution

```cpp
#include <vector>
#include <string>

using namespace std;

class Solution {
public:
    bool checkInclusion(
        string s1,
        string s2
    ) {

        if (s1.size() > s2.size()) {
            return false;
        }

        vector<int> s1Count(26, 0);
        vector<int> windowCount(26, 0);

        for (int i = 0; i < s1.size(); i++) {

            s1Count[s1[i] - 'a']++;

            windowCount[s2[i] - 'a']++;
        }

        if (s1Count == windowCount) {
            return true;
        }

        int left = 0;

        for (
            int right = s1.size();
            right < s2.size();
            right++
        ) {

            windowCount[
                s2[right] - 'a'
            ]++;

            windowCount[
                s2[left] - 'a'
            ]--;

            left++;

            if (s1Count == windowCount) {
                return true;
            }
        }

        return false;
    }
};
```

---

## Final Takeaway

**Pattern:** Fixed-Size Sliding Window

Remember:

```text
Permutation
=
Same Character Frequencies
```

Instead of generating permutations:

```text
Maintain frequency counts
for a window of size len(s1)
```

and slide across `s2`.

Result:

```text
Time Complexity  : O(n)
Space Complexity : O(1)
```

which is the optimal solution for **Permutation in String**.
