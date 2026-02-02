## Frog Minimum Cost

#### Statement

You are given `n` stones arranged in a sequence, represented by an array `height[]` of size `n`, where each element denotes the height of a stone.

A frog is initially positioned on **stone 1** (array is considered 1-indexed). The frog wants to reach the **last stone**.

From stone `i`, the frog can perform one of the following actions:

* Jump to stone `i + 1`
* Jump to stone `i + 2`

The **cost of a jump** from stone `i` to stone `j` is defined as:

```
|height[i] - height[j]|
```

Your task is to determine the **minimum total cost** required for the frog to reach the last stone.

---

#### ✔️ Constraints

* 1 ≤ n ≤ 10⁵
* 1 ≤ height[i] ≤ 10⁴
* The frog starts at stone 1
* The frog can jump either 1 or 2 stones at a time

---

## 🎯 Intuition

To reach a particular stone, the frog must come from:

* the previous stone (jump of 1), or
* the stone before that (jump of 2)

The total cost to reach a stone depends on the **minimum cost to reach earlier stones**, plus the cost of the final jump.

This optimal substructure makes the problem ideal for **Dynamic Programming**.

### 🧠 Approach

Let:

```
dp[i] = minimum cost to reach stone i
```

#### Base Cases

```
dp[1] = 0   // frog starts here, no cost
```

#### Transition

For every stone `i ≥ 2`:

```
dp[i] = min(
    dp[i-1] + |height[i] - height[i-1]|,
    dp[i-2] + |height[i] - height[i-2]|   (if i > 2)
)
```

The answer is `dp[n]`.

---

## ✅ Java Solution

### Bottom-Up DP

```java
class FrogMinCost {
    public static int minCost(int[] height) {
        int n = height.length;
        int[] dp = new int[n];
        dp[1] = Math.abs(height[1] - height[0]);

        for(int i = 2; i < n; i++) {
            int jump1 = Math.abs(height[i - 1] - height[i]) + dp[i - 1];
            int jump2 = Math.abs(height[i - 2] - height[i]) + dp[i - 2];
            dp[i] = Math.min(jump1, jump2);
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {
        int[] height = {10, 30, 40, 20};
        System.out.println(minCost(height)); // Output: 30
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)

<br>

### Space Optimized Bottom Up DP

```java
class FrogMinCost {
    public static int minCost(int[] height) {
        int n = height.length;
        if(n <= 1) return 0;

        int prev2 = 0; // dp[0]
        int prev1 = Math.abs(height[1] - height[0]); // dp[1]

        for(int i = 2; i < n; i++) {
            int oneStep = prev1 + Math.abs(height[i] - height[i - 1]);
            int twoStep = prev2 + Math.abs(height[i] - height[i - 2]);
            int curr = Math.min(oneStep, twoStep); // dp[i]
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }

    public static void main(String[] args) {
        int[] height = {10, 30, 40, 20};
        System.out.println(minCost(height)); // Output: 30
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)
