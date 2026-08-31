# Spiral Matrix

**LeetCode Problem:** https://leetcode.com/problems/spiral-matrix/

## Problem Statement

Given an `m x n` matrix, return all elements of the matrix in **spiral order**.

A spiral traversal follows this pattern:

```text
→ Right
↓ Down
← Left
↑ Up
```

and continues moving inward layer by layer until all elements have been visited. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
matrix =
[
 [1,2,3],
 [4,5,6],
 [7,8,9]
]
```

### Output

```text
[1,2,3,6,9,8,7,4,5]
```

---

## Example 2

### Input

```text
matrix =
[
 [1,2,3,4],
 [5,6,7,8],
 [9,10,11,12]
]
```

### Output

```text
[1,2,3,4,8,12,11,10,9,5,6,7]
```

---

# Understanding The Problem

Given:

```text
1  2  3
4  5  6
7  8  9
```

We must traverse:

```text
1 → 2 → 3
          ↓
4    5    6
↑         ↓
7 ← 8 ← 9
```

Result:

```text
[1,2,3,6,9,8,7,4,5]
```

Notice that we visit the matrix **layer by layer**.

Think of it like peeling an onion:

```text
Outer Layer
    ↓
Inner Layer
    ↓
Center
```

:contentReference[oaicite:1]{index=1}

---

# Key Observation

At any moment, the unvisited portion of the matrix can be represented using four boundaries:

```text
top
bottom
left
right
```

Example:

```text
top = 0
bottom = rows - 1

left = 0
right = cols - 1
```

These boundaries define the current rectangle that still needs processing. :contentReference[oaicite:2]{index=2}

---

# The Four Traversals

For each layer, we perform four traversals:

### 1. Left → Right

Traverse the top row.

```text
→ → →
```

---

### 2. Top → Bottom

Traverse the right column.

```text
↓
↓
↓
```

---

### 3. Right → Left

Traverse the bottom row.

```text
← ← ←
```

---

### 4. Bottom → Top

Traverse the left column.

```text
↑
↑
↑
```

After finishing all four sides:

```text
Shrink the boundaries inward
```

and repeat. :contentReference[oaicite:3]{index=3}

---

# Visual Walkthrough

Consider:

```text
[
 [1,2,3],
 [4,5,6],
 [7,8,9]
]
```

Initial boundaries:

```text
top = 0
bottom = 2

left = 0
right = 2
```

---

## Step 1: Traverse Top Row

```text
1 2 3
```

Result:

```text
[1,2,3]
```

Move:

```text
top += 1
```

---

## Step 2: Traverse Right Column

```text
6
9
```

Result:

```text
[1,2,3,6,9]
```

Move:

```text
right -= 1
```

---

## Step 3: Traverse Bottom Row

```text
8 7
```

Result:

```text
[1,2,3,6,9,8,7]
```

Move:

```text
bottom -= 1
```

---

## Step 4: Traverse Left Column

```text
4
```

Result:

```text
[1,2,3,6,9,8,7,4]
```

Move:

```text
left += 1
```

---

Current boundaries:

```text
top = 1
bottom = 1

left = 1
right = 1
```

Only one element remains:

```text
5
```

Final Result:

```text
[1,2,3,6,9,8,7,4,5]
```

---

# Why Do We Need Boundary Checks?

Consider a matrix with only one row:

```text
[1,2,3]
```

After traversing the top row:

```text
[1,2,3]
```

There is no bottom row left.

Without proper checks, we may visit elements again.

Therefore before traversing:

```text
Bottom Row
```

we check:

```python
if top <= bottom
```

Before traversing:

```text
Left Column
```

we check:

```python
if left <= right
```

These checks prevent duplicate visits in:

- Single row matrices
- Single column matrices
- Odd-sized matrices

:contentReference[oaicite:4]{index=4}

---

# Algorithm

Initialize:

```python
top = 0
bottom = rows - 1

left = 0
right = cols - 1
```

While:

```python
top <= bottom and left <= right
```

perform:

### Traverse Top Row

```python
left → right
```

Then:

```python
top += 1
```

---

### Traverse Right Column

```python
top → bottom
```

Then:

```python
right -= 1
```

---

### Traverse Bottom Row

If:

```python
top <= bottom
```

Traverse:

```python
right → left
```

Then:

```python
bottom -= 1
```

---

### Traverse Left Column

If:

```python
left <= right
```

Traverse:

```python
bottom → top
```

Then:

```python
left += 1
```

Repeat until boundaries cross. :contentReference[oaicite:5]{index=5}

---

# Complexity Analysis

Let:

```text
m = number of rows
n = number of columns
```

Every cell is visited exactly once.

### Time Complexity

```text
O(m × n)
```

---

### Space Complexity

Ignoring the output array:

```text
O(1)
```

We only store:

```text
top
bottom
left
right
```

as extra variables. :contentReference[oaicite:6]{index=6}

---

# Approach Comparison

| Approach | Time | Space |
|-----------|---------|---------|
| Visited Matrix Simulation | O(m × n) | O(m × n) |
| Boundary Traversal (Optimal) | O(m × n) | O(1) |

The boundary approach is preferred because it avoids maintaining a separate visited matrix. :contentReference[oaicite:7]{index=7}

---

# Key Takeaway

The core idea is:

```text
Process One Layer
        ↓
Shrink Boundaries
        ↓
Process Next Layer
```

Maintain four boundaries:

```text
top
bottom
left
right
```

and repeatedly traverse:

```text
Right
↓
Down
↓
Left
↓
Up
```

until all elements are visited.

This pattern is commonly known as:

```text
Boundary Traversal
```

or

```text
Shrinking Rectangle Technique
```

and appears frequently in matrix-based interview problems. :contentReference[oaicite:8]{index=8}

---

# Python Solution

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:

        result = []

        top = 0
        bottom = len(matrix) - 1

        left = 0
        right = len(matrix[0]) - 1

        while top <= bottom and left <= right:

            # Left -> Right
            for col in range(left, right + 1):
                result.append(matrix[top][col])

            top += 1

            # Top -> Bottom
            for row in range(top, bottom + 1):
                result.append(matrix[row][right])

            right -= 1

            # Right -> Left
            if top <= bottom:
                for col in range(right, left - 1, -1):
                    result.append(matrix[bottom][col])

                bottom -= 1

            # Bottom -> Top
            if left <= right:
                for row in range(bottom, top - 1, -1):
                    result.append(matrix[row][left])

                left += 1

        return result
```

---

# Java Solution

```java
import java.util.*;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {

        List<Integer> result = new ArrayList<>();

        int top = 0;
        int bottom = matrix.length - 1;

        int left = 0;
        int right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {

            for (int col = left; col <= right; col++) {
                result.add(matrix[top][col]);
            }

            top++;

            for (int row = top; row <= bottom; row++) {
                result.add(matrix[row][right]);
            }

            right--;

            if (top <= bottom) {

                for (int col = right; col >= left; col--) {
                    result.add(matrix[bottom][col]);
                }

                bottom--;
            }

            if (left <= right) {

                for (int row = bottom; row >= top; row--) {
                    result.add(matrix[row][left]);
                }

                left++;
            }
        }

        return result;
    }
}
```

---

# C++ Solution

```cpp
#include <vector>

using namespace std;

class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {

        vector<int> result;

        int top = 0;
        int bottom = matrix.size() - 1;

        int left = 0;
        int right = matrix[0].size() - 1;

        while (top <= bottom && left <= right) {

            for (int col = left; col <= right; col++) {
                result.push_back(matrix[top][col]);
            }

            top++;

            for (int row = top; row <= bottom; row++) {
                result.push_back(matrix[row][right]);
            }

            right--;

            if (top <= bottom) {

                for (int col = right; col >= left; col--) {
                    result.push_back(matrix[bottom][col]);
                }

                bottom--;
            }

            if (left <= right) {

                for (int row = bottom; row >= top; row--) {
                    result.push_back(matrix[row][left]);
                }

                left++;
            }
        }

        return result;
    }
};
```

---

## Final Takeaway

**Pattern:** Matrix Traversal + Boundary Contraction

Remember:

```text
Top Row
Right Column
Bottom Row
Left Column
```

After each traversal:

```text
Shrink the boundary
```

This guarantees:

```text
Every element is visited exactly once
```

with:

```text
Time Complexity  : O(m × n)
Space Complexity : O(1)
```

making it the optimal solution for Spiral Matrix.
