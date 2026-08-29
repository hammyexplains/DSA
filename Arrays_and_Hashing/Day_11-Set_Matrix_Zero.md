# Set Matrix Zeroes

**LeetCode Problem:** https://leetcode.com/problems/set-matrix-zeroes/

## Problem Statement

Given an `m × n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`.

The modification must be done **in-place**. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
matrix =
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

### Output

```text
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

### Explanation

The element at:

```text
matrix[1][1] = 0
```

Therefore:

```text
Entire row 1 → 0
Entire column 1 → 0
```

---

## Example 2

### Input

```text
matrix =
[
 [0,1,2,0],
 [3,4,5,2],
 [1,3,1,5]
]
```

### Output

```text
[
 [0,0,0,0],
 [0,4,5,0],
 [0,3,1,0]
]
```

---

# Understanding the Problem

Suppose we have:

```text
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

The moment we find:

```text
0
```

we must make:

```text
Entire Row = 0
Entire Column = 0
```

Result:

```text
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

---

# Why Can't We Update Immediately?

Consider:

```text
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

If we immediately start converting rows and columns to zero:

```text
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

Now we see many new zeroes.

If we continue processing these new zeroes, we would incorrectly turn the entire matrix into zeroes.

Therefore:

```text
We must first identify all original zero positions.
```

Only then should we modify the matrix. :contentReference[oaicite:1]{index=1}

---

# Approach 1: Extra Row and Column Arrays

## Intuition

Keep track of:

```text
Which rows contain a zero
Which columns contain a zero
```

using two arrays.

---

## Step 1

Create:

```python
rows = [False] * m
cols = [False] * n
```

---

## Step 2

Traverse the matrix.

Whenever:

```text
matrix[i][j] == 0
```

mark:

```python
rows[i] = True
cols[j] = True
```

---

## Example

```text
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

After traversal:

```text
rows = [False, True, False]
cols = [False, True, False]
```

---

## Step 3

Traverse again.

If:

```python
rows[i] or cols[j]
```

set:

```python
matrix[i][j] = 0
```

---

## Complexity

### Time Complexity

Two traversals:

```text
O(m × n)
```

### Space Complexity

```text
O(m + n)
```

for row and column markers. :contentReference[oaicite:2]{index=2}

---

# Optimal Approach: Use Matrix as Markers

## Key Insight

Instead of creating:

```text
rows[]
cols[]
```

we can use:

```text
First row
First column
```

as marker storage.

---

## Marker Meaning

If:

```text
matrix[i][0] == 0
```

then:

```text
Row i must become zero.
```

If:

```text
matrix[0][j] == 0
```

then:

```text
Column j must become zero.
```

This eliminates extra arrays. :contentReference[oaicite:3]{index=3}

---

# The Challenge

Consider:

```text
matrix[0][0]
```

This cell belongs to:

```text
First row
and
First column
```

How do we know whether:

```text
Row 0 should become zero?
```

or

```text
Column 0 should become zero?
```

We need two additional flags:

```python
firstRowZero
firstColZero
```

to remember whether the first row or first column originally contained a zero. :contentReference[oaicite:4]{index=4}

---

# Step-by-Step Algorithm

---

## Step 1: Check First Row

```python
firstRowZero = any(matrix[0][j] == 0 for j in range(cols))
```

---

## Step 2: Check First Column

```python
firstColZero = any(matrix[i][0] == 0 for i in range(rows))
```

---

## Step 3: Mark Rows and Columns

Traverse from:

```text
(1,1)
```

to avoid touching the first row and column.

Whenever:

```text
matrix[i][j] == 0
```

mark:

```python
matrix[i][0] = 0
matrix[0][j] = 0
```

---

## Example

Initial:

```text
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

After marking:

```text
[
 [1,0,1],
 [0,0,1],
 [1,1,1]
]
```

Notice:

```text
Row 1 marked
Column 1 marked
```

---

## Step 4: Zero Based on Markers

Traverse again from:

```text
(1,1)
```

If:

```python
matrix[i][0] == 0
or
matrix[0][j] == 0
```

then:

```python
matrix[i][j] = 0
```

---

## Step 5: Handle First Row

If:

```python
firstRowZero == True
```

set the entire first row to zero.

---

## Step 6: Handle First Column

If:

```python
firstColZero == True
```

set the entire first column to zero.

---

# Dry Run

Input:

```text
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

---

### Marking Phase

```text
[
 [1,0,1],
 [0,0,1],
 [1,1,1]
]
```

---

### Zeroing Phase

```text
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

---

### Final Answer

```text
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

---

# Why Does This Work?

The first row and first column act as a memory board.

Instead of storing:

```text
rows[]
cols[]
```

we reuse existing matrix cells.

Thus:

```text
Extra Space = O(1)
```

while preserving all necessary information. :contentReference[oaicite:5]{index=5}

---

# Complexity Analysis

## Time Complexity

First pass:

```text
O(m × n)
```

Second pass:

```text
O(m × n)
```

Total:

```text
O(m × n)
```

---

## Space Complexity

Only two variables:

```text
firstRowZero
firstColZero
```

Therefore:

```text
O(1)
```

extra space. :contentReference[oaicite:6]{index=6}

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Row & Column Arrays | O(m × n) | O(m + n) |
| First Row & Column Markers | O(m × n) | O(1) |

---

# Key Takeaway

This problem teaches a powerful interview pattern:

```text
Use the input itself as storage.
```

Instead of creating extra arrays:

```text
rows[]
cols[]
```

we reuse:

```text
matrix[i][0]
matrix[0][j]
```

as markers.

The crucial idea is:

```text
First Row + First Column
=
In-place Marker Arrays
```

This converts:

```text
O(m + n) space
```

into:

```text
O(1) space
```

while maintaining:

```text
O(m × n) time
```

---

# Python Solution (Optimal)

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:

        rows = len(matrix)
        cols = len(matrix[0])

        firstRowZero = any(matrix[0][j] == 0 for j in range(cols))
        firstColZero = any(matrix[i][0] == 0 for i in range(rows))

        for i in range(1, rows):
            for j in range(1, cols):

                if matrix[i][j] == 0:
                    matrix[i][0] = 0
                    matrix[0][j] = 0

        for i in range(1, rows):
            for j in range(1, cols):

                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0

        if firstRowZero:
            for j in range(cols):
                matrix[0][j] = 0

        if firstColZero:
            for i in range(rows):
                matrix[i][0] = 0
```

---

# Java Solution (Optimal)

```java
class Solution {
    public void setZeroes(int[][] matrix) {

        int rows = matrix.length;
        int cols = matrix[0].length;

        boolean firstRowZero = false;
        boolean firstColZero = false;

        for (int j = 0; j < cols; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
            }
        }

        for (int i = 0; i < rows; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
            }
        }

        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {

                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {

                if (matrix[i][0] == 0 ||
                    matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        if (firstRowZero) {
            for (int j = 0; j < cols; j++) {
                matrix[0][j] = 0;
            }
        }

        if (firstColZero) {
            for (int i = 0; i < rows; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}
```

---

# C++ Solution (Optimal)

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {

        int rows = matrix.size();
        int cols = matrix[0].size();

        bool firstRowZero = false;
        bool firstColZero = false;

        for (int j = 0; j < cols; j++) {
            if (matrix[0][j] == 0)
                firstRowZero = true;
        }

        for (int i = 0; i < rows; i++) {
            if (matrix[i][0] == 0)
                firstColZero = true;
        }

        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {

                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {

                if (matrix[i][0] == 0 ||
                    matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        if (firstRowZero) {
            for (int j = 0; j < cols; j++) {
                matrix[0][j] = 0;
            }
        }

        if (firstColZero) {
            for (int i = 0; i < rows; i++) {
                matrix[i][0] = 0;
            }
        }
    }
};
```

---

## Final Takeaway

**Pattern:** Matrix Traversal + In-Place Marking

Remember:

```text
If a row/column needs future processing,
store the information inside the matrix itself.
```

For this problem:

```text
First Row  → Column Markers
First Column → Row Markers
```

Result:

```text
Time Complexity  : O(m × n)
Space Complexity : O(1)
```

which is the optimal solution.
