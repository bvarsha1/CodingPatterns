## Symmetric Tree

#### Statement

Given the root of a binary tree, check whether it is a symmetric tree. A symmetric tree refers to a tree that is a mirror of itself, i.e., symmetric around its root.

---

#### ✔️ Constraints

* The tree contains nodes in the range `[1, 500]`
* `−10³ ≤ Node.data ≤ 10³`

---

## 🎯 Intuition

A binary tree is **symmetric** if:

* The **left subtree** is a mirror image of the **right subtree**

That means:

* Left child of the left subtree must match the **right child** of the right subtree
* Right child of the left subtree must match the **left child** of the right subtree
* Corresponding node values must be equal

### Key idea (BFS – Pairwise Comparison)

Instead of comparing the whole tree level-by-level, we compare **nodes in pairs**:

1. Start by comparing `root.left` and `root.right`
2. Push nodes into a queue **in mirrored order**
3. Always dequeue **two nodes at a time** and check:

   * Both are `null` → OK, continue
   * One is `null` → ❌ not symmetric
   * Values differ → ❌ not symmetric

Why BFS works well here:

* Queue naturally preserves **pairwise symmetry**
* Easy to compare mirror positions

---

## ✅ Java Solution (BFS – Queue Based)

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

public class Solution {
  public static boolean isSymmetric(TreeNode<Integer> root) {
    if (root == null) return true;

    Queue<TreeNode<Integer>> q = new LinkedList<>();
    q.offer(root.left);
    q.offer(root.right);

    while (!q.isEmpty()) {
      TreeNode<Integer> left = q.poll();
      TreeNode<Integer> right = q.poll();

      // both null → symmetric at this position
      if (left == null && right == null) continue;

      // only one null → not symmetric
      if (left == null || right == null) return false;

      // values must match
      if (!left.data.equals(right.data)) return false;

      // push children in mirrored order
      q.offer(left.left);
      q.offer(right.right);
      q.offer(left.right);
      q.offer(right.left);
    }

    return true;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is visited once

* **Space Complexity:** `O(n)`
  * Queue can hold up to a full level of the tree
