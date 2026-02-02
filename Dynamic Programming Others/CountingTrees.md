## **Counting Trees**

#### Statement

You are given an integer `N`. You need to count the number of **distinct binary search trees (BSTs)** that can be formed using `N` nodes numbered from `1` to `N`.

For example:

* If `N = 3`, the number of BSTs = `5`
* If `N = 4`, the number of BSTs = `14`

---

#### ✔️ Constraints

* `1 ≤ N ≤ 100`
* Each node has a **unique value** from `1` to `N`
* A valid BST satisfies: **left subtree < root < right subtree**

---

## 🎯 Intuition

The problem is a classic **Catalan number** problem:

* Each number from `1` to `N` can be the **root** of a BST.
* If a number `i` is chosen as root:

  * Left subtree has `i-1` nodes
  * Right subtree has `N-i` nodes
* The total number of BSTs is the **sum over all choices of root**, multiplying the number of BSTs in left and right subtrees.

This is a perfect **Dynamic Programming / recursion** problem.

### 🧠 Approach (Dynamic Programming)

1. Let `dp[n]` represent the number of BSTs that can be formed with `n` nodes.
2. Base case:

```
dp[0] = 1  // empty tree counts as one BST
dp[1] = 1  // single node
```

3. Recurrence:

```
dp[n] = sum(dp[i-1] * dp[n-i]) for i = 1 to n
```

Explanation:

* Choose `i` as root
* Left subtree has `i-1` nodes → `dp[i-1]` possibilities
* Right subtree has `n-i` nodes → `dp[n-i]` possibilities
* Multiply left × right and sum over all roots

4. The final answer is:

```
dp[N]
```

---

## ✅ Java Solution

```java
import java.util.*;

public class CountingTrees {

    public static int countBSTs(int N) {
        int[] dp = new int[N + 1];
        dp[0] = 1; // empty tree
        dp[1] = 1; // single node

        for (int n = 2; n <= N; n++) {
            dp[n] = 0;
            for (int root = 1; root <= n; root++) {
                dp[n] += dp[root - 1] * dp[n - root];
            }
        }

        return dp[N];
    }

    public static void main(String[] args) {
        System.out.println(countBSTs(3)); // Output: 5
        System.out.println(countBSTs(4)); // Output: 14
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(N²)` — two nested loops
* **Space Complexity:** `O(N)` — for the `dp` array