## Ladder Problem

#### Statement

You are given a ladder with `n` steps and an integer `K`. Starting from the ground (step `0`), you want to reach the top of the ladder (step `n`).

At each move, you may climb **at most `K` steps**. Your task is to compute the **total number of distinct ways** to reach the top of the ladder.

---

#### ✔️ Constraints

* 0 ≤ n ≤ 10⁵
* 1 ≤ K ≤ 10⁵
* The answer fits within a 64-bit integer

---

## 🎯 Intuition

To reach step `n`, the last jump could have been:

* from step `n - 1`
* from step `n - 2`
* …
* from step `n - K`

So the number of ways to reach step `n` is the **sum of ways** to reach the previous `K` steps.

This gives a natural recurrence relation.

### 🧠 Approach

#### Recurrence Relation

Let:

```
dp[i] = number of ways to reach step i
```

Then:

```
dp[i] = dp[i-1] + dp[i-2] + ... + dp[i-K]
```

Base case:

```
dp[0] = 1   // one way to stay at the ground
```

This is a **generalized Fibonacci problem**.

#### Optimization Insight

A direct DP implementation would be **O(n × K)**, which is too slow for large inputs.

We can optimize using a **sliding window sum**:

* Maintain the sum of the last `K` dp values
* Add the newest value
* Remove the value that slides out of the window

This reduces time complexity to **O(n)**.

---

## ✅ Java Solution (Optimized DP)

### Top-down + Memoization

```java
class LadderProblem {
    public static long countWays(int n, int k, long[] dp) {
        if(n == 0) return 1;
        if(n < 0) return 0;
        
        if(dp[n] != 0) return dp[n];

        for(int jump = 1; jump <= k; jump++) {
            dp[n] += countWays(n - jump, k, dp);
        }

        return dp[n];
    }

    public static void main(String[] args) {
        int n = 5;
        int k = 2;
        long[] dp = new long[n + 1];
        System.out.println(countWays(n, k, dp)); // Output: 8
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n x k)
* **Space Complexity:** O(n) (dp + recursion stack)

<br>

### Bottom-up (Iterative)

```java
class LadderProblem {
    public static long countWays(int n, int k) {
        long[] dp = new long[n + 1];
        dp[0] = 1;

        for (int i = 1; i <= n; i++) {
            for(int jump = 1; jump <= k; jump++) {
                if(i - jump >= 0) {
                    dp[i] += dp[i - jump];
                }
            }
        }

        return dp[n];
    }

    public static void main(String[] args) {
        int n = 5;
        int k = 2;
        System.out.println(countWays(n, k)); // Output: 8
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n x k)
* **Space Complexity:** O(n) (dp)

<br>

### Bottom-up (Iterative) - Optimized

```java
class LadderProblem {
    public static long countWays(int n, int k) {
        long[] dp = new long[n + 1];
        dp[0] = dp[1] = 1;

        for(int i = 2; i <=k; i++) {
            dp[i] = 2 * dp[i - 1];
        }

        for(int i = k + 1; i <= n; i++) {
            dp[i] = 2 * dp[i - 1] - dp[i - k - 1];
        }

        return dp[n];
    }

    public static void main(String[] args) {
        int n = 5;
        int k = 2;
        System.out.println(countWays(n, k)); // Output: 8
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)