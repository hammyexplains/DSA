# Longest Repeating Character Replacement

**LeetCode Problem:** https://leetcode.com/problems/longest-repeating-character-replacement/

## Problem Statement

You are given a string `s` consisting of uppercase English letters and an integer `k`.

You can choose any character in the string and replace it with any other uppercase English character at most `k` times.

Return the length of the **longest substring** that can be made to contain the same character after performing at most `k` replacements.

---

## Example 1

### Input

```text
s = "ABAB"
k = 2
```

### Output

```text
4
```

### Explanation

We can replace:

```text
A B A B
```

with:

```text
A A A A
```

by replacing the two `B`s.

Therefore, the longest valid substring has length:

```text
4
```

---

## Example 2

### Input

```text
s = "AABABBA"
k = 1
```

### Output

```text
4
```

### Explanation

Consider:

```text
"ABAB"
```

We can replace one `B`:

```text
A B A B
↓
A A A B
```

So the length is:

```text
4
```

A longer substring would require more than one replacement.

---

# Understanding the Problem

The key is to understand what makes a substring valid.

Suppose we have:

```text
"AAAB"
```

The most frequent character is:

```text
A → 3 times
```

The substring has length:

```text
4
```

Therefore, the number of characters that need to be replaced is:

```text
4 - 3 = 1
```

If:

```text
k = 1
```

then this substring is valid.

We can replace:

```text
B → A
```

and get:

```text
"AAAA"
```

---

# Key Formula

For any current window:

```text
window_length = right - left + 1
```

Let:

```text
max_frequency
```

be the frequency of the most common character in the window.

Then the number of replacements required is:

```text
window_length - max_frequency
```

The window is valid if:

```text
window_length - max_frequency <= k
```

This is the most important formula in this problem.

---

# Why Sliding Window?

The problem asks for:

```text
Longest substring
```

and gives us a constraint:

```text
At most k replacements
```

This is a classic **Sliding Window** problem.

We maintain a window:

```text
[left ... right]
```

and expand it using `right`.

If the window becomes invalid, move `left` forward until it becomes valid again.

---

# Example

Consider:

```text
s = "AABABBA"
k = 1
```

Suppose our window is:

```text
"AABA"
```

Character frequencies:

```text
A → 3
B → 1
```

Window length:

```text
4
```

Most frequent character:

```text
A → 3
```

Required replacements:

```text
4 - 3 = 1
```

Since:

```text
1 <= k
```

the window is valid.

---

Now consider:

```text
"AABAB"
```

Frequencies:

```text
A → 3
B → 2
```

Window length:

```text
5
```

Required replacements:

```text
5 - 3 = 2
```

But:

```text
2 > k
```

So the window is invalid.

We need to shrink it from the left.

---

# Approach: Sliding Window + Frequency Array

Since the problem contains only uppercase English letters:

```text
A-Z
```

there are only:

```text
26
```

possible characters.

We can therefore use:

```python
count = [0] * 26
```

instead of a HashMap.

The index represents a character:

```text
0  → A
1  → B
2  → C
...
25 → Z
```

---

# Algorithm

Initialize:

```python
left = 0
max_freq = 0
result = 0
count = [0] * 26
```

Then iterate through the string using `right`.

---

## Step 1: Add Current Character

For:

```text
s[right]
```

increase its frequency:

```python
count[ord(s[right]) - ord('A')] += 1
```

---

## Step 2: Track Maximum Frequency

Update:

```python
max_freq = max(max_freq, count[index])
```

This represents the frequency of the most common character we have encountered in the current window.

---

## Step 3: Check If Window Is Invalid

Calculate:

```text
window_length - max_freq
```

If:

```text
window_length - max_freq > k
```

the window requires too many replacements.

Shrink it:

```python
count[ord(s[left]) - ord('A')] -= 1
left += 1
```

---

## Step 4: Update Answer

Once the window is valid:

```python
result = max(
    result,
    right - left + 1
)
```

---

# Dry Run

Consider:

```text
s = "AABABBA"
k = 1
```

---

### Window: `"A"`

```text
A → 1
```

Window length:

```text
1
```

Required replacements:

```text
1 - 1 = 0
```

Valid.

Answer:

```text
1
```

---

### Window: `"AA"`

```text
A → 2
```

Required replacements:

```text
2 - 2 = 0
```

Valid.

Answer:

```text
2
```

---

### Window: `"AAB"`

Frequencies:

```text
A → 2
B → 1
```

Window length:

```text
3
```

Required replacements:

```text
3 - 2 = 1
```

Valid.

Answer:

```text
3
```

---

### Window: `"AABA"`

Frequencies:

```text
A → 3
B → 1
```

Window length:

```text
4
```

Required replacements:

```text
4 - 3 = 1
```

Valid.

Answer:

```text
4
```

---

### Add another B

Window:

```text
"AABAB"
```

Frequencies:

```text
A → 3
B → 2
```

Window length:

```text
5
```

Required replacements:

```text
5 - 3 = 2
```

But:

```text
2 > 1
```

Invalid.

Shrink from the left.

---

# Important Insight About `max_freq`

You may notice that in the code we don't always decrease `max_freq` when the left side of the window moves.

For example:

```python
count[left_char] -= 1
left += 1
```

but we don't do:

```python
max_freq -= 1
```

This is intentional.

We allow `max_freq` to represent the **maximum frequency we've seen during the window expansion**, even if the current window's actual maximum frequency becomes slightly smaller.

This does not affect the correctness of the answer.

Why?

If the window appears valid because of an old `max_freq`, the calculated window length can only cause us to keep a window that corresponds to a previously achievable maximum length. When the window truly needs to grow beyond what is possible, the condition will eventually force the left pointer forward.

This lets us avoid repeatedly calculating the maximum frequency across all 26 characters.

---

# Why Not Recalculate `max_freq`?

We could calculate:

```python
max(count)
```

after every window update.

Because there are only 26 characters, this is still technically constant time:

```text
O(26) = O(1)
```

However, keeping `max_freq` incrementally makes the solution simpler and more efficient.

---

# Brute Force Approach

One possible brute-force solution is:

1. Generate every substring.
2. Count the frequency of each character.
3. Find the most frequent character.
4. Calculate how many replacements are needed.
5. Check whether replacements are `<= k`.

This involves many repeated calculations.

---

## Complexity

Depending on implementation, the brute-force approach can take approximately:

```text
O(n²)
```

or worse if frequencies are recomputed inefficiently.

The sliding window avoids this repeated work.

---

# Complexity Analysis

Let:

```text
n = len(s)
```

## Time Complexity

The `right` pointer moves from:

```text
0 → n - 1
```

The `left` pointer also moves forward and never moves backward.

Therefore, each character is processed a constant number of times.

```text
O(n)
```

---

## Space Complexity

We store frequencies for only:

```text
26 characters
```

Therefore:

```text
O(26)
```

which simplifies to:

```text
O(1)
```

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Brute Force | O(n²) or worse | O(1) |
| Sliding Window + Frequency Array | O(n) | O(1) |

---

# Pattern Recognition

This problem is an important example of:

```text
Sliding Window
+
Frequency Counting
+
At Most K Changes
```

Whenever you see something like:

```text
Longest substring
+
At most K modifications
```

you should consider:

```text
Sliding Window
```

---

# Important Formula

The most important thing to remember from this problem is:

```text
Required Replacements
=
Window Length - Most Frequent Character Frequency
```

Therefore:

```text
Window is valid if:

Window Length - Max Frequency <= k
```

Or:

```text
(right - left + 1) - max_freq <= k
```

---

# Python Solution

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:

        count = [0] * 26

        left = 0
        max_freq = 0
        result = 0

        for right in range(len(s)):

            index = ord(s[right]) - ord('A')

            count[index] += 1

            max_freq = max(
                max_freq,
                count[index]
            )

            while (
                right - left + 1 - max_freq > k
            ):

                left_index = (
                    ord(s[left]) - ord('A')
                )

                count[left_index] -= 1

                left += 1

            result = max(
                result,
                right - left + 1
            )

        return result
```

---

# Java Solution

```java
class Solution {
    public int characterReplacement(
        String s,
        int k
    ) {

        int[] count = new int[26];

        int left = 0;
        int maxFreq = 0;
        int result = 0;

        for (int right = 0;
             right < s.length();
             right++) {

            int index =
                s.charAt(right) - 'A';

            count[index]++;

            maxFreq = Math.max(
                maxFreq,
                count[index]
            );

            while (
                right - left + 1 - maxFreq > k
            ) {

                int leftIndex =
                    s.charAt(left) - 'A';

                count[leftIndex]--;

                left++;
            }

            result = Math.max(
                result,
                right - left + 1
            );
        }

        return result;
    }
}
```

---

# C++ Solution

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    int characterReplacement(
        string s,
        int k
    ) {

        vector<int> count(26, 0);

        int left = 0;
        int maxFreq = 0;
        int result = 0;

        for (int right = 0;
             right < s.size();
             right++) {

            int index =
                s[right] - 'A';

            count[index]++;

            maxFreq = max(
                maxFreq,
                count[index]
            );

            while (
                right - left + 1 - maxFreq > k
            ) {

                int leftIndex =
                    s[left] - 'A';

                count[leftIndex]--;

                left++;
            }

            result = max(
                result,
                right - left + 1
            );
        }

        return result;
    }
};
```

---

## Final Takeaway

**Pattern:** Sliding Window + Frequency Counting

The key question to ask is:

> How many characters would I need to replace to make the entire current window the same character?

The answer is:

```text
Window Length - Most Frequent Character Frequency
```

If:

```text
Window Length - Max Frequency <= k
```

the window is valid.

Otherwise:

```text
Move the left pointer
```

This gives:

```text
Time Complexity  : O(n)
Space Complexity : O(1)
```

and is the optimal approach for **Longest Repeating Character Replacement**.
