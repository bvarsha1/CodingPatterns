
## Height of Binary Tree After Subtree Removal Queries

#### Statement

We are given the root of a binary tree with `n` nodes and an array, queries, of size `m`. Each query represents the root of a subtree that should be removed from the tree. The task here is to determine the height of the binary tree after each query, i.e., once a subtree is removed. We'll store the updated heights against each query in an array and return it.

> Note: A tree’s height is the number of edges in the longest path from the root to any leaf node in the tree.

A few points to be considered:
* All the values in the tree are unique.
* It is guaranteed that queries[i] will not be equal to the value of the root.
* The queries are independent, so the tree returns to its initial state after each query.

---

#### ✔️ Constraints

* `2 ≤ n ≤ 500`
* `1 ≤ Node.data ≤ n`
* `m == queries.length`
* `1 ≤ m ≤ min(n, 400)`
* `1 ≤ queries[i] ≤ n`
* `queries[i] ≠ root.data`

---

## 🎯 Intuition (Key Insight)

Removing a subtree rooted at node **X** means:

👉 **All paths that go through X are gone**

So the new tree height must come from:

* Some **other subtree at the same depth (level)** as X
* That does **not use X’s subtree**

### Naive approach ❌

For each query:

* Remove subtree
* Recompute height
  ➡️ `O(n)` per query → too slow

---

### 💡 Optimized Strategy (Precomputation)

We solve this in **one DFS traversal**, then answer each query in **O(1)**.

### During DFS, we compute:

1. **height[node]**

   * Height of subtree rooted at this node

2. **level[node]**

   * Depth (level) of node from root

3. **top2h[level]**

   * For each level, store:

     * `top2h[level][0]` → largest subtree height at this level
     * `top2h[level][1]` → second largest subtree height at this level

---

### 🧠 Why `top2h` is needed?

When removing node `q`:

* If `q` had the **tallest subtree** at its level
  → we must use the **second tallest**
* Otherwise
  → tallest remains unaffected

---

### 📐 Height Formula After Removal

```
newHeight = bestRemainingHeightAtLevel + level - 1
```

Why?

* `bestRemainingHeightAtLevel` → height **below that level**
* `level - 1` → edges **above that level**

---

## ✅ Java Solution (Efficient – O(n + m))

```java
import java.util.*;
import ds_v1.BinaryTree.TreeNode;

// Definiton of a binary tree node class
// class TreeNode<T> {
//     T data;
//     TreeNode<T> left;
//     TreeNode<T> right;

//     TreeNode(T data) {
//         this.data = data;
//         this.left = null;
//         this.right = null;
//     }
// }

class Solution {
  public int[] heightsAfterQueries(TreeNode<Integer> root, int[] queries) {

    // height of subtree rooted at each node
    int[] heights = new int[100001];

    // level (depth) of each node
    int[] levels = new int[100001];
    Arrays.fill(levels, -1);

    // for each level → top 2 subtree heights
    int[][] top2h = new int[100001][2];

    // single DFS precomputation
    preCompute(root, 0, heights, levels, top2h);

    int[] ans = new int[queries.length];

    for (int i = 0; i < queries.length; i++) {
      int qNode = queries[i];

      int height = heights[qNode];
      int level = levels[qNode];

      // choose best height excluding this subtree
      int remainingHeight =
        (height == top2h[level][0])
          ? top2h[level][1]
          : top2h[level][0];

      ans[i] = remainingHeight + level - 1;
    }

    return ans;
  }

  public static int preCompute(
      TreeNode<Integer> node,
      int level,
      int[] hs,
      int[] ls,
      int[][] top2h
  ) {
    if (node == null) return 0;

    int left = preCompute(node.left, level + 1, hs, ls, top2h);
    int right = preCompute(node.right, level + 1, hs, ls, top2h);

    int height = 1 + Math.max(left, right);

    hs[node.data] = height;
    ls[node.data] = level;

    // update top 2 heights for this level
    if (height > top2h[level][0]) {
      top2h[level][1] = top2h[level][0];
      top2h[level][0] = height;
    } else if (height > top2h[level][1]) {
      top2h[level][1] = height;
    }

    return height;
  }
}
```

#### ⏱ Complexity Analysis

* **Precomputation:** `O(n)`
* **Each Query:** `O(1)`
* **Total Time:** `O(n + m)`
* **Space Complexity:** `O(n)`
