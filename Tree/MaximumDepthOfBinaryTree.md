## Maximum Depth of Binary Tree

#### Statement

You are given the root of a binary tree, and your task is to determine the maximum depth of this tree. The maximum depth of a binary tree is determined by the count of nodes found on the longest pathway from the root node to the farthest leaf node.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range `[1, 500]`
* `−100 ≤ Node.data ≤ 100`

---

## 🎯 Intuition

The **maximum depth** of a binary tree is simply the **height of the tree measured in number of nodes**.

Key observations:

* If the tree is empty (`root == null`), the depth is `0`
* Otherwise:

  * Recursively compute the depth of the **left subtree**
  * Recursively compute the depth of the **right subtree**
  * Take the **maximum** of the two and add `1` for the current node

This is a classic **DFS (Depth-First Search)** problem where each recursive call answers:

> “What is the maximum depth starting from this node?”

---

## ✅ Java Solution (DFS)

```java
import ds_v1.BinaryTree.TreeNode;

public class Solution {
    public static int maxDepth(TreeNode<Integer> root) {
        // base case
        if (root == null) return 0;

        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);

        return 1 + Math.max(leftDepth, rightDepth);
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  → Every node is visited exactly once

* **Space Complexity:** `O(h)`
  → Due to recursion stack, where `h` is the height of the tree
  → Worst case (skewed tree): `O(n)`
  → Best case (balanced tree): `O(log n)`