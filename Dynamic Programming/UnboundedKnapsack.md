## Unbounded Knapsack

#### Statement

You are given:

* An array `weight[]` of positive integers where `weight[i]` represents the weight of the ith item
* An array `value[]` of positive integers where `value[i]` represents the value of the ith item
* A positive integer `capacity`

Each item may be chosen **an unlimited number of times**. The goal is to compute the **maximum total value** achievable without exceeding `capacity`.

---

#### ✔️ Constraints

* 1 ≤ `n` ≤ 1000 (number of item types)
* 1 ≤ `weight[i]`, `value[i]` ≤ 10⁴
* 1 ≤ `capacity` ≤ 10⁴
* Items can be reused any number of times

---

## 🎯 Intuition

This is similar to classical knapsack, except:

➡ In **0/1 Knapsack**, each item can be used **at most once**
➡ In **Unbounded Knapsack**, each item can be reused **infinitely**

This changes the DP transition strategy.

Key idea:

If `dp[c]` stores maximum value for capacity `c`, then:

```
dp[c] = max(dp[c], dp[c - weight[i]] + value[i])
```

Notice we stay at same `i`, meaning we can reuse item `i`.

### Approach (DP Tabulation - 1D)

We use a 1D DP array of length `capacity + 1`.

Steps:

1. Initialize `dp[c] = 0` for all capacities
2. For each item:
3. Try filling capacities from `weight[i]` to `capacity`
4. Update using recurrence

Because we traverse **capacity forward**, reuse is naturally allowed.

---

## Java Solution

```java
class UnboundedKnapsack {
    public static int unboundedKnapsack(int[] weight, int[] value, int capacity) {
        int n = weight.length;
        int[] dp = new int[capacity + 1];

        for (int i = 0; i < n; i++) {
            for (int c = weight[i]; c <= capacity; c++) {
                dp[c] = Math.max(dp[c], dp[c - weight[i]] + value[i]);
            }
        }

        return dp[capacity];
    }

    public static void main(String[] args) {
        int[] weight = {2, 3, 4};
        int[] value  = {40, 50, 60};
        int capacity = 5;

        System.out.println(unboundedKnapsack(weight, value, capacity)); // Output: 90
    }
}
```

#### 📝 Example Walkthrough

Input:

```
weight = [2, 3, 4]
value  = [40, 50, 60]
capacity = 5
```

Optimal:

* Pick item with weight 2 twice → value = 40 + 40 = 80 (capacity used = 4)
* Remaining capacity = 1 (can't fit weight 2 or 3 or 4)
* Better option is: weight 3 + weight 2 = 50 + 40 = 90

Answer = `90`

#### ⏱ Complexity

* Time Complexity: O(n × capacity)
* Space Complexity: O(capacity)

---

### Notes

* Uses **forward capacity traversal** allowing unlimited reuse
* If you traverse **backwards**, it becomes **0/1 Knapsack**
* Works for maximizing value under fixed weight capacity
* Variants include: minimizing cost, counting ways, generating combinations
