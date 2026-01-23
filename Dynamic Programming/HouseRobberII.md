## House Robber II

#### **Statement**

A professional robber plans to rob some houses along a street. These houses are arranged in a circle, which means that the first and the last house are neighbors. The robber cannot rob adjacent houses because they have security alarms installed.

Following the constraints mentioned above and given an integer array `money` representing the amount of money in each house, return the maximum amount the robber can steal without alerting the police.

---

#### ✔️ Constraints

* `1 ≤ money.length ≤ 10^3`
* `0 ≤ money[i] ≤ 10^3`

---

## 🎯 Intuition

This is a variation of the classic **House Robber (Linear DP)** problem, but with a circular twist.

Because the first and last house are neighbors, **we cannot rob both**.
Therefore, split into two independent subproblems:

1. **Rob houses from index `[1 .. n-1]`** (exclude first)
2. **Rob houses from index `[0 .. n-2]`** (exclude last)

Compute both cases and take the maximum.
Each linear case follows the recurrence:

```
newRob = max(rob1 + money[i], rob2)
```

Where:

* `rob1` = dp[i-2]
* `rob2` = dp[i-1]

---

## ✅ Java Solution

```java
import java.util.*;

class HouseRobber {
  public static int houseRobber(int[] money) {
    if(money.length == 1) return money[0];
    return Math.max(money[0], Math.max(rob(money, 1, money.length), rob(money, 0, money.length - 1)));
  }
  
  public static int rob(int[] money, int s, int e) {
      int rob1 = 0, rob2 = 0;
      
      for(int i = s; i < e; i++) {
        int newRob = Math.max(rob1 + money[i], rob2);
        rob1 = rob2;
        rob2 = newRob;
      }
      
      return rob2;
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)