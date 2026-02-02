## Maximum Non-Adjacent Sum

#### Statement

You are given an array of **positive integers**.

Your task is to find the **maximum possible sum** of a subset of elements such that **no two chosen elements are adjacent** in the array.

In other words, you must select elements in a way that maximizes the sum while ensuring that no two selected elements are next to each other.

---

#### ✔️ Constraints

* 1 ≤ n ≤ 10⁵
* 1 ≤ arr[i] ≤ 10⁴
* The array contains only positive integers

---

## 🎯 Intuition

At every index, we face a choice:

* **Include** the current element → then we **cannot include** the previous one
* **Exclude** the current element → then the result depends on the previous maximum

So the decision at each position depends on the best decisions made earlier.

This is a classic **Dynamic Programming** problem.

### 🧠 Approach

Let:

```
dp[i] = maximum sum considering elements up to index i
```

At index `i`, we have two options:

1. Do not take `arr[i]`

   ```
   dp[i] = dp[i-1]
   ```
2. Take `arr[i]`

   ```
   dp[i] = dp[i-2] + arr[i]
   ```

So the recurrence becomes:

```
dp[i] = max(dp[i-1], dp[i-2] + arr[i])
```

#### Base Cases

```
dp[0] = arr[0]
dp[1] = max(arr[0], arr[1])
```

The answer will be `dp[n-1]`.

---

## ✅ Java Solution (Bottom-Up DP)

### Classic Bottom Up DP solution

```java
class MaxNonAdjacentSum {
    public static int maxSum(int[] arr) {
        int n = arr.length;
        if (n == 0) return 0;
        if (n == 1) return arr[0];

        int[] dp = new int[n];
        dp[0] = arr[0];
        dp[1] = Math.max(arr[0], arr[1]);

        for (int i = 2; i < n; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + arr[i]);
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {
        int[] arr = {12, 9, 6, 14};
        System.out.println(maxSum(arr)); // Output: 32
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)

<br>

### Bottom Up - Space Optimized Solution

```java
class MaxNonAdjacentSum {
    public static int maxSum(int[] arr) {
        int n = arr.length;
        if (n == 0) return 0;
        if (n == 1) return arr[0];

        int prev2 = arr[0];
        int prev1 = Math.max(arr[0], arr[1]);

        for (int i = 2; i < n; i++) {
            int current = Math.max(prev1, prev2 + arr[i]);
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }

    public static void main(String[] args) {
        int[] arr = {12, 9, 6, 14};
        System.out.println(maxSum(arr)); // Output: 32
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)

---

## 📌 Notes

* Also known as the `House Robber problem`
* Can be optimized to O(1) space
* Greedy approach does not work due to overlapping choices
* Variant of 0/1 Knapsack, but without the capacity or weight limit, but a adjacency constraint