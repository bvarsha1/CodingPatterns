## Coin Change

#### **Statement**

Given an integer `total` that represents the target amount of money and a list of integers `coins` that represents different coin denominations, find the minimum number of coins required to make up the total amount. If it’s impossible to achieve the target amount using the given coins, return `-1`. If the target amount is 0, return `0`.

> **Note:** You can assume that we have an infinite number of each kind of coin.

---

#### ✔️ Constraints

* 1 ≤ `coins.length` ≤ 12
* 1 ≤ `coins[i]` ≤ 10^4
* 0 ≤ `total` ≤ 900

---

## 🎯 Intuition

This is a classic **Dynamic Programming (Unbounded Knapsack)** problem.

Idea:

* Define `dp[x]` as the **minimum number of coins** needed to form amount `x`
* Initialize all values as large (`∞`), except `dp[0] = 0` (0 coins needed to make amount 0)
* For each coin, update all reachable totals:

```
for coin in coins:
    for x from coin to total:
        dp[x] = min(dp[x], dp[x - coin] + 1)
```

Finally:

* If `dp[total]` is still `∞`, return `-1`
* Else return `dp[total]`

---

## ✅ Java Solution

#### Solution 1: GenAI suggested
```java
import java.util.Arrays;

class Solution {
    public int coinChange(int[] coins, int total) {
        int[] dp = new int[total + 1];
        Arrays.fill(dp, total + 1);
        dp[0] = 0;

        for (int coin : coins) {
            for (int x = coin; x <= total; x++) {
                dp[x] = Math.min(dp[x], dp[x - coin] + 1);
            }
        }

        return dp[total] > total ? -1 : dp[total];
    }
}
```

#### Solution 2 : My intuition
```java
import java.util.*;
public class CoinChange{
  public static int coinChange(int [] coins, int total) {
    int[] dp = new int[total + 1];
    dp[0] = 0;
    
    for(int i = 1; i <= total; i++) {
      dp[i] = Integer.MAX_VALUE;
      for(int coin : coins) {
        if(i - coin >= 0 && dp[i - coin] != Integer.MAX_VALUE) {
          dp[i] = Math.min(dp[i], dp[i - coin] + 1);
        }
      }
    }
    
    return dp[total] != Integer.MAX_VALUE ? dp[total] : -1;
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n × total)
  (where `n` = number of coins)
* **Space Complexity:** O(total)