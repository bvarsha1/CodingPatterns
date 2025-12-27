## Validate Binary Search Tree

#### Statement

Given the root of a binary tree, check whether it is a valid binary search tree (BST).

A binary tree is a valid BST if for every node:

* The left subtree of a node contains only nodes with keys **less than** the node’s key.
* The right subtree of a node contains only nodes with keys **greater than** the node’s key.
* Both the left and right subtrees are valid BSTs.

---

#### ✔️ Constraints

* `−10^4 ≤ Node.data ≤ 10^4`
* The tree contains nodes in the range `[1, 500]`

---

## 🎯 Intuition

The key challenge in validating a BST is **not just comparing a node with its immediate children**, but ensuring that:

* **All nodes in the left subtree** are smaller than the current node
* **All nodes in the right subtree** are larger than the current node

To guarantee this globally, we:

* Pass down a **valid range** `(min, max)` for every node
* Initially, the range is `(-∞, +∞)`
* For each node:

  * Its value **must lie strictly inside** the allowed range
  * The left child’s range becomes `(min, node.data)`
  * The right child’s range becomes `(node.data, max)`

If **any node violates its allowed range**, the tree is not a valid BST.

This approach ensures correctness even for deep subtrees.

---

## ✅ Java Solution

```java
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

import java.util.*;
import ds_v1.BinaryTree.TreeNode;

public class Solution {
    public static boolean validateBst(TreeNode<Integer> root) {
        return isValid(root, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }
  
    public static boolean isValid(
            TreeNode<Integer> node,
            int left,
            int right
    ) {
        if (node == null) return true;
        
        // node value must lie strictly within the valid range
        if (node.data <= left || node.data >= right) return false;
        
        // validate left and right subtrees with updated bounds
        return isValid(node.left, left, node.data) &&
               isValid(node.right, node.data, right);
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  Every node is visited exactly once.

* **Space Complexity:** `O(h)`
  Recursive stack space, where `h` is the height of the tree
  *Worst case:* `O(n)` (skewed tree)
  *Best case:* `O(log n)` (balanced tree)
