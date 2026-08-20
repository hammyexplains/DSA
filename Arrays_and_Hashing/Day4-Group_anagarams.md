# Group Anagrams

**LeetCode Problem:** https://leetcode.com/problems/group-anagrams/

## Problem Statement

Given an array of strings, group the strings that are anagrams of each other.

Anagrams are strings that contain the same characters with the same frequencies, but the characters may appear in a different order.

Return the groups in any order.

---

## Example

### Input

```text
["eat", "tea", "tan", "ate", "nat", "bat"]
```

### Output

```text
[
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
]
```

### Explanation

After sorting each string:

```text
eat -> aet
tea -> aet
ate -> aet

tan -> ant
nat -> ant

bat -> abt
```

Strings that produce the same sorted string are anagrams of each other.

---

# Approach

We can use a HashMap / Dictionary to group the anagrams.

### Step 1

Create an empty dictionary.

```text
dictionary = {}
```

The key will be the sorted version of the string.

The value will be a list containing all strings that have the same sorted form.

---

### Step 2

Iterate through every string.

For each string:

```text
"eat"
```

sort its characters:

```text
"aet"
```

Use `"aet"` as the key.

---

### Step 3

If the key does not exist, create an empty list.

```text
"aet" -> []
```

Then add the original string:

```text
"aet" -> ["eat"]
```

---

### Step 4

Continue processing the remaining strings.

For example:

```text
tea -> aet
ate -> aet
```

The dictionary becomes:

```text
"aet" -> ["eat", "tea", "ate"]
```

Similarly:

```text
"ant" -> ["tan", "nat"]
"abt" -> ["bat"]
```

---

### Step 5

Return all the dictionary values.

---

# Dry Run

Input:

```text
["eat", "tea", "tan", "ate", "nat", "bat"]
```

Dictionary after processing each string:

```text
eat
aet -> ["eat"]
```

```text
tea
aet -> ["eat", "tea"]
```

```text
tan
ant -> ["tan"]
```

```text
ate
aet -> ["eat", "tea", "ate"]
```

```text
nat
ant -> ["tan", "nat"]
```

```text
bat
abt -> ["bat"]
```

Final dictionary:

```text
{
    "aet": ["eat", "tea", "ate"],
    "ant": ["tan", "nat"],
    "abt": ["bat"]
}
```

The values of the dictionary are the required answer.

---

# Complexity Analysis

Let:

- `n` = number of strings
- `k` = maximum length of a string

For every string, we sort its characters.

Sorting a string of length `k` takes:

```text
O(k log k)
```

For `n` strings:

```text
O(n * k log k)
```

### Time Complexity

```text
O(n * k log k)
```

### Space Complexity

```text
O(n * k)
```

This includes the space required to store the grouped strings and the dictionary keys.

---

# Key Takeaway

The important idea is:

> Anagrams become identical when their characters are sorted.

For example:

```text
eat -> aet
tea -> aet
ate -> aet
```

Therefore, the sorted string can be used as a **key** in a HashMap / Dictionary.

This is a common **Hashing + Sorting** pattern.

---

# Python

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:

        dico = {}

        for word in strs:

            key = "".join(sorted(word))

            if key not in dico:
                dico[key] = []

            dico[key].append(word)

        return list(dico.values())
```

---

# Java

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

        HashMap<String, List<String>> dico = new HashMap<>();

        for (String word : strs) {

            char[] chars = word.toCharArray();

            Arrays.sort(chars);

            String key = new String(chars);

            if (!dico.containsKey(key)) {
                dico.put(key, new ArrayList<>());
            }

            dico.get(key).add(word);
        }

        return new ArrayList<>(dico.values());
    }
}
```

---

# C++

```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {

        unordered_map<string, vector<string>> dico;

        for (string word : strs) {

            string key = word;

            sort(key.begin(), key.end());

            dico[key].push_back(word);
        }

        vector<vector<string>> result;

        for (auto& pair : dico) {
            result.push_back(pair.second);
        }

        return result;
    }
};
```


## Key Takeaway

The main idea behind this problem is to find a **common key** for all anagrams.

Since anagrams contain the same characters, sorting the characters gives us the same result for every string in the same group.

```text
eat -> aet
tea -> aet
ate -> aet
