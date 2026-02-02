## Rod Cutting Problem

#### Statement

You are given a rod of length `n` and an array `price[]`, where `price[i]` represents the price of a rod piece of length `i + 1`.

You may cut the rod into **any number of smaller pieces** of integer lengths. Each piece can be sold individually according to the given prices.

Your task is to determine the **maximum total profit** that can be obtained by cutting (or not cutting) the rod and selling the resulting pieces.

---

#### ✔️ Constraints

* 1 ≤ n ≤ 1000
* 1 ≤ price[i] ≤ 10⁴
* `price.length = n`
* The rod can be cut into any number of pieces
* Pieces must have integer lengths

---

## 🎯 Intuition

For a rod of length `n`, we have two choices:

* Sell the rod **as a whole**
* Cut the rod into smaller pieces and sell each piece separately

The key observation is:

> Every possible cut splits the rod into two smaller rods, and the best solution for `n` depends on the best solutions of smaller lengths.

This makes the problem a classic **Dynamic Programming** problem.

### 🧠 Approach

Let:

```
dp[i] = maximum profit obtainable from a rod of length i
```

#### Recurrence Relation

For every length `i`, try all possible first cuts:

```
dp[i] = max(
    price[i-1],
    dp[1] + dp[i-1],
    dp[2] + dp[i-2],
    ...
    dp[i-1] + dp[1]
)
```

In simpler form:

```
dp[i] = max(price[j] + dp[i - j - 1])  for all j from 0 to i-1
```

This ensures we explore **all possible ways** of cutting the rod.

### 🧩 Example

Given:

```
n = 8
price = [1, 5, 8, 9, 10, 17, 17, 20]
```

Some possible strategies:

* Sell whole rod → 20
* Cut into two pieces of length 4 → 9 + 9 = 18
* Cut into lengths 6 and 2 → 17 + 5 = 22 ✅ (maximum)

Output:

```
22
```

---

## ✅ Java Solution

### Bottom-Up DP - Unbounded Knapsack

```java
class RodCutting {
    public static int cutRod(int[] price, int n) {
        int[] dp = new int[n + 1];
        dp[0] = 0;

        for (int i = 1; i <= n; i++) {
            int maxVal = Integer.MIN_VALUE;
            for (int j = 0; j < i; j++) {
                maxVal = Math.max(maxVal, price[j] + dp[i - j - 1]);
            }
            dp[i] = maxVal;
        }

        return dp[n];
    }

    public static void main(String[] args) {
        int[] price = {1, 5, 8, 9, 10, 17, 17, 20};
        int n = 8;
        System.out.println(cutRod(price, n)); // Output: 22
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n²)
* **Space Complexity:** O(n)

---

## 📌 Notes

* This is an **Unbounded Knapsack** problem
* You can use the same piece length multiple times
* If all prices were equal, smaller cuts could yield higher profit
* This problem is often used to introduce:
  * DP optimization
  * Unbounded knapsack patterns
  * Bottom-up reasoning
