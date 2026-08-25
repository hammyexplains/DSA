# Valid Sudoku

**LeetCode Problem:** https://leetcode.com/problems/valid-sudoku/

## Problem Statement

Determine if a `9 x 9` Sudoku board is valid.

Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes must contain the digits `1-9` without repetition.

### Important Note

A Sudoku board can be:

```text
Valid but not solvable
```

We only need to verify that the current board does not violate any Sudoku rules. We are **not solving** the Sudoku. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
[
["5","3",".",".","7",".",".",".","."],
["6",".",".","1","9","5",".",".","."],
[".","9","8",".",".",".",".","6","."],
["8",".",".",".","6",".",".",".","3"],
["4",".",".","8",".","3",".",".","1"],
["7",".",".",".","2",".",".",".","6"],
[".","6",".",".",".",".","2","8","."],
[".",".",".","4","1","9",".",".","5"],
[".",".",".",".","8",".",".","7","9"]
]
```

### Output

```text
true
```

---

## Example 2

### Input

```text
[
["8","3",".",".","7",".",".",".","."],
["6",".",".","1","9","5",".",".","."],
[".","9","8",".",".",".",".","6","."],
["8",".",".",".","6",".",".",".","3"],
["4",".",".","8",".","3",".",".","1"],
["7",".",".",".","2",".",".",".","6"],
[".","6",".",".",".",".","2","8","."],
[".",".",".","4","1","9",".",".","5"],
[".",".",".",".","8",".",".","7","9"]
]
```

### Output

```text
false
```

### Why?

The digit `8` appears twice in the top-left `3 x 3` box. Therefore the board is invalid. :contentReference[oaicite:1]{index=1}

---

# Understanding The Rules

We only care about filled cells.

```text
'.'
```

means the cell is empty.

Ignore those cells completely.

For every digit we encounter, we must ensure:

```text
1. Not already present in its row
2. Not already present in its column
3. Not already present in its 3×3 box
```

If any of the above conditions fail:

```text
Return False
```

Otherwise:

```text
Return True
```

---

# Brute Force Thought Process

For every cell:

1. Check the entire row.
2. Check the entire column.
3. Check the entire box.

This would work, but we'd repeatedly scan the same rows and columns.

Not efficient.

---

# Optimal Approach: Hash Sets

We need a quick way to answer:

```text
Have I already seen this number?
```

A HashSet gives:

```text
O(1)
```

average lookup time.

We'll maintain:

```text
rows[9]
cols[9]
boxes[9]
```

Where:

```text
rows[i]
```

stores all numbers seen in row `i`.

```text
cols[j]
```

stores all numbers seen in column `j`.

```text
boxes[k]
```

stores all numbers seen in box `k`.

---

# Visualizing The Boxes

Sudoku contains 9 boxes:

```text
+-------+-------+-------+
| Box 0 | Box 1 | Box 2 |
+-------+-------+-------+
| Box 3 | Box 4 | Box 5 |
+-------+-------+-------+
| Box 6 | Box 7 | Box 8 |
+-------+-------+-------+
```

We need a formula to identify which box a cell belongs to.

---

# The Box Formula

For a cell:

```text
(row, col)
```

Box index:

```python
box = (row // 3) * 3 + (col // 3)
```

This is the most important concept in this problem. :contentReference[oaicite:2]{index=2}

---

## Example

### Cell (1,1)

```text
row // 3 = 0
col // 3 = 0

box = 0 * 3 + 0
     = 0
```

Belongs to:

```text
Box 0
```

---

### Cell (4,7)

```text
row // 3 = 1
col // 3 = 2

box = 1 * 3 + 2
     = 5
```

Belongs to:

```text
Box 5
```

---

### Cell (8,8)

```text
row // 3 = 2
col // 3 = 2

box = 2 * 3 + 2
     = 8
```

Belongs to:

```text
Box 8
```

---

# Algorithm

Traverse every cell.

For each digit:

### Step 1

Skip empty cells.

```python
if board[r][c] == ".":
    continue
```

---

### Step 2

Compute box index.

```python
box = (r // 3) * 3 + (c // 3)
```

---

### Step 3

Check duplicates.

If digit already exists in:

```text
row set
OR
column set
OR
box set
```

return:

```text
False
```

---

### Step 4

Insert digit into:

```text
row set
column set
box set
```

---

### Step 5

After traversing the entire board:

```text
Return True
```

---

# Dry Run

Suppose we encounter:

```text
board[0][0] = "5"
```

Check:

```text
rows[0]
cols[0]
boxes[0]
```

Not found.

Insert:

```text
rows[0]  -> {5}
cols[0]  -> {5}
boxes[0] -> {5}
```

---

Next:

```text
board[0][1] = "3"
```

Check:

```text
rows[0]
cols[1]
boxes[0]
```

Not found.

Insert.

Continue.

If later another:

```text
5
```

appears in the same row, column, or box:

```text
Return False immediately.
```

---

# Complexity Analysis

The board size is fixed:

```text
9 × 9 = 81 cells
```

We visit each cell exactly once.

### Time Complexity

```text
O(81)
```

Which simplifies to:

```text
O(1)
```

because the board size never changes. :contentReference[oaicite:3]{index=3}

---

### Space Complexity

We store:

```text
9 row sets
9 column sets
9 box sets
```

Maximum:

```text
81 digits
```

Therefore:

```text
O(81)
```

or:

```text
O(1)
```

for fixed Sudoku size. :contentReference[oaicite:4]{index=4}

---

# Key Takeaway

This problem is not about Sudoku.

It is about:

```text
HashSet + Matrix Traversal
```

The most important concepts are:

```text
1. Detect duplicates efficiently
2. Track rows, columns and boxes separately
3. Understand box indexing
```

Remember this formula:

```python
box = (row // 3) * 3 + (col // 3)
```

This formula appears in almost every optimal Sudoku solution. :contentReference[oaicite:5]{index=5}

---

# Python Solution

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:

        rows = [set() for _ in range(9)]
        cols = [set() for _ in range(9)]
        boxes = [set() for _ in range(9)]

        for r in range(9):
            for c in range(9):

                value = board[r][c]

                if value == ".":
                    continue

                box = (r // 3) * 3 + (c // 3)

                if (
                    value in rows[r] or
                    value in cols[c] or
                    value in boxes[box]
                ):
                    return False

                rows[r].add(value)
                cols[c].add(value)
                boxes[box].add(value)

        return True
```

---

# Java Solution

```java
import java.util.HashSet;

class Solution {
    public boolean isValidSudoku(char[][] board) {

        HashSet<Character>[] rows = new HashSet[9];
        HashSet<Character>[] cols = new HashSet[9];
        HashSet<Character>[] boxes = new HashSet[9];

        for (int i = 0; i < 9; i++) {
            rows[i] = new HashSet<>();
            cols[i] = new HashSet<>();
            boxes[i] = new HashSet<>();
        }

        for (int r = 0; r < 9; r++) {

            for (int c = 0; c < 9; c++) {

                char value = board[r][c];

                if (value == '.') {
                    continue;
                }

                int box = (r / 3) * 3 + (c / 3);

                if (
                    rows[r].contains(value) ||
                    cols[c].contains(value) ||
                    boxes[box].contains(value)
                ) {
                    return false;
                }

                rows[r].add(value);
                cols[c].add(value);
                boxes[box].add(value);
            }
        }

        return true;
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
    bool isValidSudoku(vector<vector<char>>& board) {

        vector<unordered_set<char>> rows(9);
        vector<unordered_set<char>> cols(9);
        vector<unordered_set<char>> boxes(9);

        for (int r = 0; r < 9; r++) {

            for (int c = 0; c < 9; c++) {

                char value = board[r][c];

                if (value == '.') {
                    continue;
                }

                int box = (r / 3) * 3 + (c / 3);

                if (
                    rows[r].count(value) ||
                    cols[c].count(value) ||
                    boxes[box].count(value)
                ) {
                    return false;
                }

                rows[r].insert(value);
                cols[c].insert(value);
                boxes[box].insert(value);
            }
        }

        return true;
    }
};
```

---

## Final Takeaway

**Pattern:** Hash Set + Matrix Traversal

Whenever a problem asks:

```text
Check duplicates efficiently
inside rows, columns, or groups
```

think about:

```text
HashSet
```

And whenever Sudoku is involved, remember:

```python
box = (row // 3) * 3 + (col // 3)
```

That single formula is the key to solving this problem efficiently.
