# Best Time to Buy and Sell Stock

**LeetCode Problem:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

## Problem Statement

You are given an array `prices` where:

```text
prices[i]
```

represents the stock price on day `i`.

You may choose:

```text
One day to buy
and
One later day to sell
```

Return the maximum profit you can achieve.

If no profit is possible, return:

```text
0
```

You cannot sell before buying. :contentReference[oaicite:0]{index=0}

---

## Example 1

### Input

```text
prices = [7,1,5,3,6,4]
```

### Output

```text
5
```

### Explanation

```text
Buy at 1
Sell at 6

Profit = 6 - 1 = 5
```

---

## Example 2

### Input

```text
prices = [7,6,4,3,1]
```

### Output

```text
0
```

### Explanation

Prices keep decreasing.

No profitable transaction exists. :contentReference[oaicite:1]{index=1}

---

# Understanding The Problem

We need to find:

```text
Maximum Profit
=
Sell Price - Buy Price
```

with one important rule:

```text
Buy must happen before Sell
```

Example:

```text
prices = [7,1,5,3,6,4]
```

Possible profits:

```text
Buy 7 Sell 1 = -6

Buy 1 Sell 5 = 4

Buy 1 Sell 6 = 5  ← Maximum
```

Answer:

```text
5
```

---

# Brute Force Approach

For every day:

Assume it is the buy day.

Try selling on every future day.

```python
for i in range(n):
    for j in range(i + 1, n):
        profit = prices[j] - prices[i]
```

Keep track of the maximum profit.

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

Too slow for large inputs. :contentReference[oaicite:2]{index=2}

---

# Key Insight

Suppose today's price is:

```text
6
```

To maximize profit:

```text
We want the cheapest price before today.
```

We don't need to check all previous days repeatedly.

Instead, we can continuously track:

```text
Minimum price seen so far
```

and calculate:

```text
Profit if sold today
```

This leads to a one-pass solution. :contentReference[oaicite:3]{index=3}

---

# Optimal Approach: Track Minimum Price

Maintain two variables:

```python
min_price
max_profit
```

---

## min_price

Stores:

```text
Cheapest stock price seen so far
```

---

## max_profit

Stores:

```text
Best profit found so far
```

---

# Algorithm

Traverse the array once.

For each price:

### Step 1

Calculate profit if sold today.

```python
profit = price - min_price
```

---

### Step 2

Update maximum profit.

```python
max_profit = max(max_profit, profit)
```

---

### Step 3

Update minimum price.

```python
min_price = min(min_price, price)
```

Continue until the end. :contentReference[oaicite:4]{index=4}

---

# Dry Run

Input:

```text
prices = [7,1,5,3,6,4]
```

Initialize:

```text
min_price = 7
max_profit = 0
```

---

### Day 1

Price:

```text
1
```

Update minimum:

```text
min_price = 1
```

Profit:

```text
1 - 7 = -6
```

Ignore.

---

### Day 2

Price:

```text
5
```

Profit:

```text
5 - 1 = 4
```

Update:

```text
max_profit = 4
```

---

### Day 3

Price:

```text
3
```

Profit:

```text
3 - 1 = 2
```

No update.

---

### Day 4

Price:

```text
6
```

Profit:

```text
6 - 1 = 5
```

Update:

```text
max_profit = 5
```

---

### Day 5

Price:

```text
4
```

Profit:

```text
4 - 1 = 3
```

No update.

---

Final Answer:

```text
5
```

---

# Visual Intuition

For:

```text
[7,1,5,3,6,4]
```

Track the minimum price:

```text
7 → 1 → 1 → 1 → 1 → 1
```

Profit each day:

```text
0
0
4
2
5
3
```

Maximum:

```text
5
```

---

# Why This Works

At every day:

```text
Current Price
-
Lowest Previous Price
```

gives the best profit if we sell today.

By checking this for every day:

```text
We automatically find
the best buy/sell pair.
```

The minimum price always represents the best buying opportunity before the current day. :contentReference[oaicite:5]{index=5}

---

# Alternative View: Two Pointers

Think of:

```text
left  = buy day
right = sell day
```

If:

```text
prices[right] < prices[left]
```

move buy day to the cheaper price.

Otherwise:

```text
calculate profit
```

This is effectively the same logic as maintaining the minimum price. :contentReference[oaicite:6]{index=6}

---

# Complexity Analysis

## Time Complexity

Single traversal:

```text
O(n)
```

---

## Space Complexity

Only two variables:

```text
O(1)
```

This is the optimal solution. :contentReference[oaicite:7]{index=7}

---

# Approach Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n²) | O(1) |
| Min Price Tracking | O(n) | O(1) |

---

# Key Takeaway

This problem teaches an important pattern:

```text
Running Minimum
+
Running Maximum Profit
```

Instead of checking every buy-sell pair:

```text
Keep track of the cheapest price seen so far.
```

Then for every day:

```text
Profit = Current Price - Cheapest Price
```

Update the answer continuously.

---

# Python Solution

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:

        min_price = float('inf')
        max_profit = 0

        for price in prices:

            max_profit = max(
                max_profit,
                price - min_price
            )

            min_price = min(
                min_price,
                price
            )

        return max_profit
```

---

# Java Solution

```java
class Solution {
    public int maxProfit(int[] prices) {

        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for (int price : prices) {

            maxProfit = Math.max(
                maxProfit,
                price - minPrice
            );

            minPrice = Math.min(
                minPrice,
                price
            );
        }

        return maxProfit;
    }
}
```

---

# C++ Solution

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {

        int minPrice = INT_MAX;
        int maxProfit = 0;

        for (int price : prices) {

            maxProfit = max(
                maxProfit,
                price - minPrice
            );

            minPrice = min(
                minPrice,
                price
            );
        }

        return maxProfit;
    }
};
```

---

## Final Takeaway

**Pattern:** Running Minimum + Greedy

Remember:

```text
Buy at the cheapest price seen so far.
```

For every new day:

```text
Calculate profit if sold today.
```

Keep updating:

```text
Minimum Price
Maximum Profit
```

Result:

```text
Time Complexity  : O(n)
Space Complexity : O(1)
```

which is the optimal solution for **Best Time to Buy and Sell Stock**.