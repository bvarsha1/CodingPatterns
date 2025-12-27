## Invert Binary Tree

#### Statement

Given the root node of a binary tree, transform the tree by swapping each node’s left and right subtrees, thus creating a mirror image of the original tree. Return the root of the transformed tree.

---

#### ✔️ Constraints

* `0 ≤ Number of nodes in the tree ≤ 100`
* `−1000 ≤ Node.value ≤ 1000`

---

## 🎯 Intuition

To **invert a binary tree**, we simply need to **swap the left and right child of every node**.

Key observations:

* The operation is **local to each node**
* After swapping at the current node, we must also invert:

  * its left subtree
  * its right subtree
* This naturally suggests a **recursive DFS (postorder or preorder)** approach

Base case:

* If the current node is `null`, there is nothing to invert

Recursive case:

1. Swap `left` and `right`
2. Recursively invert both subtrees

This ensures the entire tree is mirrored.

---

## ✅ Java Solution (DFS – Recursive)

```java
import ds_v1.BinaryTree.TreeNode;

public class Solution {
    public static TreeNode<Integer> invertTree(TreeNode<Integer> root) {
        if (root == null) return null;

        // swap left and right
        TreeNode<Integer> temp = root.left;
        root.left = root.right;
        root.right = temp;

        // invert subtrees
        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  (each node is visited exactly once)

* **Space Complexity:** `O(h)`
  where `h` is the height of the tree due to recursion stack
  *Worst case:* `O(n)` (skewed tree)
  *Best case:* `O(log n)` (balanced tree)
