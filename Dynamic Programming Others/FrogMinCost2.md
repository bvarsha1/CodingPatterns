## **Frog Min Cost 2**

#### Statement

There are `N` stones, numbered from `1` to `N`. Each stone `i` has a height `h[i]`.

A frog starts on **Stone 1** and wants to reach **Stone N**. The frog can jump from its current stone `i` to any stone `j` such that:

```
i + 1 ≤ j ≤ i + K
```

Each jump from stone `i` to stone `j` incurs a **cost** equal to the absolute difference in heights:

```
cost = |h[i] − h[j]|
```

Find the **minimum total cost** for the frog to reach Stone `N`.

---

#### ✔️ Constraints

* `2 ≤ N ≤ 100000` — number of stones
* `1 ≤ K ≤ 100` — maximum jump distance
* `1 ≤ h[i] ≤ 10000` — height of stone `i`

---

## 🎯 Intuition

* The problem is a variant of **1D DP with range jumps**.
* The frog can jump up to `K` stones ahead, so we need to **consider all reachable stones at each step**.
* Define `dp[i]` as the **minimum cost to reach stone i**.
* Then for each stone `i`, we check all possible jumps `j = i-K` to `i-1` and pick the minimum:

> Dynamic programming over reachable jumps with cost accumulation.

### 🧠 Approach (Dynamic Programming)

1. Let `dp[i]` = **minimum cost to reach stone i**
2. Base case:

```
dp[1] = 0  // frog starts here
```

3. Transition:

```
dp[i] = min(dp[j] + |h[i] - h[j]|)   for all j where i-K ≤ j < i
```

4. Final answer:

```
dp[N]
```

---

## ✅ Java Solution

```java
import java.util.*;

public class FrogJump2 {

    public static int minCost(int K, int[] h) {
        int N = h.length;
        int[] dp = new int[N];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0; // start at first stone

        for (int i = 1; i < N; i++) {
            for (int j = Math.max(0, i - K); j < i; j++) {
                dp[i] = Math.min(dp[i], dp[j] + Math.abs(h[i] - h[j]));
            }
        }

        return dp[N - 1];
    }

    public static void main(String[] args) {
        int K = 3;
        int[] h = {10, 30, 40, 50, 20};
        System.out.println(minCost(K, h)); // Output: 30
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(N × K)` — for each stone, check up to K previous stones
* **Space Complexity:** `O(N)` — for the DP array
