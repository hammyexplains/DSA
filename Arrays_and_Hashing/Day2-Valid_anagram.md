# Valid Anagram

**LeetCode Problem:** https://leetcode.com/problems/valid-anagram/

## Problem Statement

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An **anagram** is a word or phrase formed by rearranging the letters of another word or phrase, using all the original letters exactly once.

---

## Example 1

**Input**

```text
s = "anagram"
t = "nagaram"
```

**Output**

```text
true
```

**Explanation**

Both strings contain the same characters with the same frequencies.

---

## Example 2

**Input**

```text
s = "rat"
t = "car"
```

**Output**

```text
false
```

**Explanation**

The character frequencies do not match.

---

## Intuition

To determine whether two strings are anagrams, we need to verify that:

1. Both strings have the same length.
2. Every character appears the same number of times in both strings.

A HashMap (or Dictionary) can be used to keep track of character frequencies.

---

## Approach

### Step 1

Check whether the lengths of both strings are equal.

If the lengths are different, they cannot be anagrams.

### Step 2

Traverse the first string and store the frequency of each character in a HashMap.

Example:

```text
s = "aab"
```

Frequency Map:

```text
a → 2
b → 1
```

### Step 3

Traverse the second string.

For each character:

- If the character does not exist in the map, return `false`.
- If its frequency becomes negative or is already zero, return `false`.
- Otherwise, decrease its frequency by one.

### Step 4

If the traversal completes successfully, return `true`.

---

## Dry Run

### Input

```text
s = "anagram"
t = "nagaram"
```

### Build Frequency Map

```text
a → 3
n → 1
g → 1
r → 1
m → 1
```

### Process String t

Character: `n`

```text
n → 0
```

Character: `a`

```text
a → 2
```

Character: `g`

```text
g → 0
```

Character: `a`

```text
a → 1
```

Character: `r`

```text
r → 0
```

Character: `a`

```text
a → 0
```

Character: `m`

```text
m → 0
```

All characters matched successfully.

Return:

```text
true
```

---

## Why Does This Work?

The frequency map stores how many times each character should appear.

While processing the second string:

- Every matching character decreases its count.
- If a required character is missing or appears too many times, the strings are not anagrams.

Since the lengths are equal and every frequency matches, the strings are anagrams.

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Where `n` is the length of the string.

Reason:

- One traversal for `s`
- One traversal for `t`

Total:

```text
O(n)
```

---

### Space Complexity

```text
O(k)
```

Where `k` is the number of unique characters.

In the worst case:

```text
O(n)
```

---

## Python Solution

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:

        if len(s) != len(t):
            return False

        counter = {}

        for char in s:
            counter[char] = counter.get(char, 0) + 1

        for char in t:
            if char not in counter or counter[char] == 0:
                return False

            counter[char] -= 1

        return True
```

---

## Java Solution

```java
import java.util.HashMap;

class Solution {
    public boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        HashMap<Character, Integer> counter = new HashMap<>();

        for (char c : s.toCharArray()) {
            counter.put(c, counter.getOrDefault(c, 0) + 1);
        }

        for (char c : t.toCharArray()) {

            if (!counter.containsKey(c) || counter.get(c) == 0) {
                return false;
            }

            counter.put(c, counter.get(c) - 1);
        }

        return true;
    }
}
```

---

## C++ Solution

```cpp
#include <unordered_map>
#include <string>

using namespace std;

class Solution {
public:
    bool isAnagram(string s, string t) {

        if (s.length() != t.length()) {
            return false;
        }

        unordered_map<char, int> counter;

        for (char c : s) {
            counter[c]++;
        }

        for (char c : t) {

            if (counter.find(c) == counter.end() || counter[c] == 0) {
                return false;
            }

            counter[c]--;
        }

        return true;
    }
};
```

---

## Key Takeaway

Whenever a problem asks:

- Compare frequencies of characters
- Check whether two strings contain the same characters
- Count occurrences of elements

Think about using a **HashMap / Dictionary**.

Frequency counting is one of the most common patterns in coding interviews and appears frequently in string and array problems.
