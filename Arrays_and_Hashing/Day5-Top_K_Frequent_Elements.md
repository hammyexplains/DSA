# Top K Frequent Elements

**LeetCode:** https://leetcode.com/problems/top-k-frequent-elements/

## Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

The answer may be returned in any order.

### Example

**Input:**

```text
nums = [1,1,1,2,2,3]
k = 2
```

**Output:**

```text
[1,2]
```

### Explanation

The frequency of each number is:

```text
1 -> 3 times
2 -> 2 times
3 -> 1 time
```

Therefore, the 2 most frequent elements are:

```text
[1,2]
```

---

# Approach 1: HashMap + Sorting

## Intuition

First, we count how many times each number appears using a HashMap / Dictionary.

For:

```text
[1,1,1,2,2,3]
```

we get:

```text
1 -> 3
2 -> 2
3 -> 1
```

Then, we sort the numbers based on their frequencies in descending order.

Finally, we return the first `k` numbers.

---

## Step 1: Count Frequencies

```python
freq = {}

for num in nums:
    freq[num] = freq.get(num, 0) + 1
```

For:

```text
nums = [1,1,1,2,2,3]
```

the dictionary becomes:

```text
{
    1: 3,
    2: 2,
    3: 1
}
```

---

## Step 2: Sort Based on Frequency

We can sort the dictionary items using the frequency as the sorting key.

```python
sorted_items = sorted(
    freq.items(),
    key=lambda x: x[1],
    reverse=True
)
```

The result is:

```text
[
    (1,3),
    (2,2),
    (3,1)
]
```

Here:

```text
number -> frequency
```

---

## Step 3: Take the First K Elements

If:

```text
k = 2
```

we take:

```text
[(1,3), (2,2)]
```

and return only the numbers:

```text
[1,2]
```

---

## Time Complexity

Let:

- `n` = total number of elements
- `m` = number of unique elements

Counting frequencies:

```text
O(n)
```

Sorting the unique elements:

```text
O(m log m)
```

Therefore:

```text
O(n + m log m)
```

Since `m <= n`, the worst-case complexity is:

```text
O(n log n)
```

## Space Complexity

The frequency dictionary stores `m` unique elements:

```text
O(m)
```

In the worst case:

```text
O(n)
```

---

# Approach 2: Bucket Sort

## Intuition

Instead of sorting the elements by their frequency, we can use the frequency itself as an index.

An element can appear at most `n` times.

Therefore, we can create `n + 1` buckets.

For example:

```text
nums = [1,1,1,2,2,3]
```

Frequency map:

```text
1 -> 3
2 -> 2
3 -> 1
```

We create buckets where the index represents the frequency:

```text
Frequency 0 -> []
Frequency 1 -> [3]
Frequency 2 -> [2]
Frequency 3 -> [1]
Frequency 4 -> []
Frequency 5 -> []
Frequency 6 -> []
```

So:

```text
bucket[1] -> [3]
bucket[2] -> [2]
bucket[3] -> [1]
```

---

## Step 1: Count Frequencies

```python
freq = {}

for num in nums:
    freq[num] = freq.get(num, 0) + 1
```

Result:

```text
{
    1: 3,
    2: 2,
    3: 1
}
```

---

## Step 2: Create Buckets

```python
bucket = [[] for _ in range(len(nums) + 1)]
```

If `len(nums) = 6`:

```text
bucket = [
    [],
    [],
    [],
    [],
    [],
    [],
    []
]
```

The index represents frequency.

---

## Step 3: Put Numbers Into Their Frequency Bucket

```python
for num, count in freq.items():
    bucket[count].append(num)
```

Now:

```text
bucket[1] -> [3]
bucket[2] -> [2]
bucket[3] -> [1]
```

---

## Step 4: Traverse From Highest Frequency

We start from the highest possible frequency and move down.

```python
result = []

for i in range(len(bucket) - 1, 0, -1):

    for num in bucket[i]:

        result.append(num)

        if len(result) == k:
            return result
```

For our example:

```text
bucket[6] -> []
bucket[5] -> []
bucket[4] -> []
bucket[3] -> [1]
bucket[2] -> [2]
```

Result:

```text
[1,2]
```

Once we have `k` elements, we return immediately.

---

# Why Is Bucket Sort O(n)?

There is no comparison-based sorting.

We simply:

1. Count frequencies.
2. Put each number into its frequency bucket.
3. Traverse the buckets.

Each element is processed a limited number of times.

Therefore:

```text
Time Complexity: O(n)
```

---

# Complexity Analysis

### Frequency Counting

```text
O(n)
```

### Creating Buckets

```text
O(n)
```

### Filling Buckets

```text
O(m)
```

where `m` is the number of unique elements.

Since:

```text
m <= n
```

this is:

```text
O(n)
```

### Traversing Buckets

There are at most `n + 1` buckets:

```text
O(n)
```

Therefore, the overall time complexity is:

```text
O(n)
```

---

## Space Complexity

The frequency dictionary requires:

```text
O(m)
```

The buckets require:

```text
O(n)
```

Therefore:

```text
O(n)
```

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|----------|------------------|------------------|
| HashMap + Sorting | O(n + m log m) | O(m) |
| Bucket Sort | O(n) | O(n) |

Where:

- `n` = total number of elements
- `m` = number of unique elements

In the worst case, `m = n`.

Therefore:

```text
HashMap + Sorting -> O(n log n)
Bucket Sort       -> O(n)
```

---

# Important Note About Runtime

Even though Bucket Sort has better Big-O complexity, it does not necessarily mean it will always have a lower runtime in Python.

For example, a sorting-based solution might sometimes show a lower runtime on LeetCode than a Bucket Sort solution.

This can happen because:

- Python's built-in `sorted()` is highly optimized.
- Bucket Sort creates many Python lists.
- Different test cases can have different numbers of unique elements.
- LeetCode runtime measurements can vary between submissions.

Therefore:

```text
Theoretical Complexity != Actual Runtime
```

Big-O tells us how the algorithm scales as the input becomes larger.

---

# Key Takeaway

The main pattern in this problem is:

```text
Count Frequency
      ↓
Find Highest Frequencies
      ↓
Return Top K Elements
```

There are two important approaches:

### HashMap + Sorting

Simple and easy to implement:

```text
O(n + m log m)
```

### Bucket Sort

More optimal asymptotically:

```text
O(n)
```

The important DSA concept is recognizing that when the possible frequency range is limited, we can use **Bucket Sort instead of comparison-based sorting**.

---

# Python Solutions

## HashMap + Sorting

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        freq = {}

        for num in nums:
            freq[num] = freq.get(num, 0) + 1

        sorted_items = sorted(
            freq.items(),
            key=lambda x: x[1],
            reverse=True
        )

        return [num for num, freq in sorted_items[:k]]
```

## Bucket Sort

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        freq = {}

        for num in nums:
            freq[num] = freq.get(num, 0) + 1

        bucket = [[] for _ in range(len(nums) + 1)]

        for num, count in freq.items():
            bucket[count].append(num)

        result = []

        for i in range(len(bucket) - 1, 0, -1):

            for num in bucket[i]:

                result.append(num)

                if len(result) == k:
                    return result
```

---

# Java Solutions

## HashMap + Sorting

```java
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {

        Map<Integer, Integer> freq = new HashMap<>();

        for (int num : nums) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        List<Map.Entry<Integer, Integer>> entries =
                new ArrayList<>(freq.entrySet());

        entries.sort((a, b) -> b.getValue() - a.getValue());

        int[] result = new int[k];

        for (int i = 0; i < k; i++) {
            result[i] = entries.get(i).getKey();
        }

        return result;
    }
}
```

## Bucket Sort

```java
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {

        Map<Integer, Integer> freq = new HashMap<>();

        for (int num : nums) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        List<Integer>[] bucket = new ArrayList[nums.length + 1];

        for (int i = 0; i < bucket.length; i++) {
            bucket[i] = new ArrayList<>();
        }

        for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
            bucket[entry.getValue()].add(entry.getKey());
        }

        int[] result = new int[k];
        int index = 0;

        for (int i = bucket.length - 1; i >= 0; i--) {

            for (int num : bucket[i]) {

                result[index++] = num;

                if (index == k) {
                    return result;
                }
            }
        }

        return result;
    }
}
```

---

# C++ Solutions

## HashMap + Sorting

```cpp
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {

        unordered_map<int, int> freq;

        for (int num : nums) {
            freq[num]++;
        }

        vector<pair<int, int>> items;

        for (auto& entry : freq) {
            items.push_back({entry.first, entry.second});
        }

        sort(items.begin(), items.end(),
             [](const pair<int, int>& a,
                const pair<int, int>& b) {
                 return a.second > b.second;
             });

        vector<int> result;

        for (int i = 0; i < k; i++) {
            result.push_back(items[i].first);
        }

        return result;
    }
};
```

## Bucket Sort

```cpp
#include <vector>
#include <unordered_map>

using namespace std;

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {

        unordered_map<int, int> freq;

        for (int num : nums) {
            freq[num]++;
        }

        vector<vector<int>> bucket(nums.size() + 1);

        for (auto& entry : freq) {
            bucket[entry.second].push_back(entry.first);
        }

        vector<int> result;

        for (int i = bucket.size() - 1; i >= 0; i--) {

            for (int num : bucket[i]) {

                result.push_back(num);

                if (result.size() == k) {
                    return result;
                }
            }
        }

        return result;
    }
};
```

---

## Final Takeaway

For this problem, remember these two approaches:

```text
HashMap + Sorting
O(n + m log m)
```

and:

```text
HashMap + Bucket Sort
O(n)
```

The sorting approach is simpler and practical, while Bucket Sort provides the optimal asymptotic time complexity.

**Pattern to remember:**

> When you need the top elements based on frequency and the frequency has a limited range, think about **Bucket Sort**.
