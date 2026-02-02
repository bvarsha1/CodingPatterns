## 0/1 Knapsack

#### Statement

You are given `n` items whose weights and values are known, as well as a knapsack to carry these items. The knapsack cannot carry more than a certain maximum weight, known as its capacity.

You need to maximize the total value of the items in your knapsack, while ensuring that the sum of the weights of the selected items does not exceed the capacity of the knapsack.

If there is no combination of weights whose sum is within the capacity constraint, return `0`.

**Notes:**

1.  An item may not be broken up to fit into the knapsack, i.e., an item either goes into the knapsack in its entirety or not at all.
2.  We may not add an item more than once to the knapsack.

---

#### ✔️ Constraints

* `1 ≤ capacity ≤ 1000`
* `1 ≤ values.length ≤ 500`
* `weights.length = values.length`
* `1 ≤ values[i] ≤ 1000`
* `1 ≤ weights[i] ≤ capacity`

---

## 🎯 Intuition

This is a classic **Dynamic Programming** optimization problem.

Key idea:

* For each item, we have **two choices**:

  1. **Take it** (if it fits)
  2. **Skip it**
* We want the **maximum total value** without exceeding capacity.

We define a DP table where:

```
dp[i][w] = maximum value using first i items with capacity w
```

Transition:

```
If item weight > w:
    dp[i][w] = dp[i-1][w]
Else:
    dp[i][w] = max(
        dp[i-1][w],                                 // skip item
        dp[i-1][w - weights[i]] + values[i]         // take item
    )
```

The answer is `dp[n][capacity]`.

Space can be optimized to **1D DP** since each row only depends on the previous row.

---

## ✅ Java Solution

### 2D Top-Down & Bottoms-Up solutions

```java
import java.util.*;

class FindMaxKnapsackProfit {

    // 1. Bottoms Up Approach
        
    public static int findMaxKnapsackProfitBU(int capacity, int [] weights, int [] values) {
        int[][] dp = new int[weights.length + 1][capacity + 1];

        for(int i = 1; i <= weights.length; i++) {
        for(int j = 1; j <= capacity; j++) {
            int inc = 0, exc = 0;
            if(j - weights[i - 1] >= 0) {
            inc = values[i - 1] + dp[i - 1][j - weights[i - 1]];
            }
            exc = dp[i - 1][j];
            dp[i][j] = Math.max(inc, exc);
        }
        }
        
        return dp[weights.length][capacity];
    }

    // 2. Top Down Approach

    public static int findMaxKnapsackProfitTD(int capacity, int [] weights, int [] values) {
        int[][] dp = new int[weights.length + 1][capacity + 1];

        return knapsack(capacity, weights.length, weights, values, dp);
    }
    
    public static int knapsack(int C, int N, int[] weights, int[] values, int[][] dp) {
        if(N == 0 || C == 0) return 0;
        
        if(dp[N][C] != 0) return dp[N][C];
        
        int inc = 0, exc = 0;
        if(C - weights[N - 1] >= 0) {
            inc = values[N - 1] + knapsack(C - weights[N - 1], N - 1, weights, values, dp);
        }
        exc = knapsack(C, N - 1, weights, values, dp);
        
        dp[N][C] = Math.max(inc, exc);
        
        return dp[N][C];
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** `O(n * capacity)`
* **Space Complexity:** `O(n * capacity)`

<br>

### Space Optimized solution

```java
class Solution {
    public int knapsack(int[] values, int[] weights, int capacity) {
        int n = values.length;
        int[] dp = new int[capacity + 1];
        
        for (int i = 0; i < n; i++) {
            int value = values[i];
            int weight = weights[i];
            
            // iterate backwards to avoid overwriting needed data
            for (int w = capacity; w >= weight; w--) {
                dp[w] = Math.max(dp[w], dp[w - weight] + value);
            }
        }
        
        return dp[capacity];
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** `O(n * capacity)`
* **Space Complexity:** `O(capacity)`