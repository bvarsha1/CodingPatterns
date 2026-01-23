## Climbing Stairs

#### **Statement**

You are climbing a staircase. It takes `n` steps to reach the top. Each time, you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

---

#### ✔️ Constraints

* 1 ≤ `n` ≤ 45

---

## 🎯 Intuition

This problem is a classic **Dynamic Programming (Fibonacci)** scenario.

Reason:

* To reach step `i`, you must have come from either:

  * step `i - 1` (taking 1 step), or
  * step `i - 2` (taking 2 steps)

So the recurrence becomes:

```
ways[i] = ways[i-1] + ways[i-2]
```

Base cases:

* `ways[1] = 1` (only one way: 1 step)
* `ways[2] = 2` (two ways: 1+1 or 2)

Thus, the number of ways to reach `n` is the `n`-th Fibonacci number.

---

## ✅ Java Solution

```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 2) return n;

        int prev1 = 2; // ways to reach step 2
        int prev2 = 1; // ways to reach step 1

        for (int i = 3; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)