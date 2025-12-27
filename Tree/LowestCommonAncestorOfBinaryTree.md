## Lowest Common Ancestor of a Binary Tree

#### Statement

Given the root node of a binary tree with
*n* nodes, your task is to find the **lowest common ancestor** of two of its nodes, `p` and `q`.

> **Note:** The lowest common ancestor of two nodes, `p` and `q`, is defined as the lowest node in the binary tree that has both `p` and `q` as descendants.
> A node can also be a descendant of itself. For example, if `q` is a descendant of `p`, then `p` is the lowest common ancestor of `p` and `q`.

---

#### ✔️ Constraints

* `2 ≤ n ≤ 500`
* All `Node.data` are unique
* `p != q`
* Both `p` and `q` exist in the tree

---

## 🎯 Intuition

This problem is best solved using **DFS (postorder traversal)**.

Think of it this way:

* At every node, ask:

  * Does my **left subtree** contain `p` or `q`?
  * Does my **right subtree** contain `p` or `q`?
* There are **three important cases**:

1. **Current node is `p` or `q`**
   → This node could be the LCA
2. **One node found in left subtree and the other in right subtree**
   → Current node is the **LCA**
3. **Both nodes found in the same subtree**
   → LCA lies **below**, return that result upward

The recursion naturally bubbles up the correct node without extra data structures.

---

## ✅ Java Solution (DFS)

```java
import ds_v1.BinaryTree.TreeNode;

public class Solution {
    public static TreeNode<Integer> lowestCommonAncestor(
            TreeNode<Integer> root,
            TreeNode<Integer> p,
            TreeNode<Integer> q
    ) {
        // base case
        if (root == null || root == p || root == q) {
            return root;
        }

        TreeNode<Integer> left = lowestCommonAncestor(root.left, p, q);
        TreeNode<Integer> right = lowestCommonAncestor(root.right, p, q);

        // if p and q are found in different subtrees
        if (left != null && right != null) {
            return root;
        }

        // otherwise return the non-null subtree result
        return left != null ? left : right;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)` — each node is visited once
* **Space Complexity:** `O(h)` — recursion stack (`h` = tree height)
