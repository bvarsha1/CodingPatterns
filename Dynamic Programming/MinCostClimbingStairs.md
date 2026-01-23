## Min Cost Climbing Stairs

#### **Statement**

You are given an integer array, `cost`, where `cost[i]` represents the cost of stepping onto the i<sup>th</sup> stair. After paying the cost of the stair you land on, you may climb either one or two steps forward. You may begin your climb from step `0` or step `1` without incurring any initial cost.

Return the minimum total cost required to reach the position just beyond the last stair (the “top”).

---

#### ✔️ Constraints

* 2 ≤ `cost.length` ≤ 1000
* 0 ≤ `cost[i]` ≤ 999

---

## 🎯 Intuition

This is a classic dynamic programming problem.

At each step `i`, you can come from:

* step `i-1` (pay `cost[i-1]`)
* step `i-2` (pay `cost[i-2]`)

Define DP:

* `dp[i]` = minimum cost to reach step `i`

Goal is to reach step `n` (just beyond last stair), and you do **not** pay cost at `n`.

Transition:

```
dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
```

Answer: `dp[n]`

---

## 🧩 Approach

1. Let `n = cost.length`
2. Create `dp` array of size `n+1`
3. Initialize `dp[0] = dp[1] = 0`
4. For `i = 2` to `n`:

   * `dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`
5. Return `dp[n]`

We only need last two values → can reduce to O(1) space.

---

## ✅ Java Solution

```java
public class MinCostClimbingStairs {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        int prev2 = 0; // dp[0]
        int prev1 = 0; // dp[1]

        for (int i = 2; i <= n; i++) {
            int curr = Math.min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
}
```

## ⏱️ Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)` using rolling DP variables
