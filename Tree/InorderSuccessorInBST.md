## Inorder Successor in BST

#### Statement

You are given the root node of a binary search tree and a specific node `p`. Your task is to return the inorder successor of this `p` node. If there is no inorder successor of the given node, return `NULL`.

> **Note:** The inorder successor of `p` is the node with the smallest value greater than `p.data` in the binary search tree.

---

#### ✔️ Constraints

* The tree contains nodes in the range `[1,500]`
* `−10^4 ≤ Node.data ≤ 10^4`
* All nodes have unique values
* `p` exists in the tree

---

## 🎯 Intuition

Because this is a **Binary Search Tree**, we can use its ordering property:

* **Left subtree** → smaller values
* **Right subtree** → larger values

There are **two cases** for finding the inorder successor of node `p`:

### 🔹 Case 1: `p` has a right subtree

* The successor is the **leftmost node** in `p.right`
* Why? Because it is the smallest value greater than `p`

### 🔹 Case 2: `p` has no right subtree

* Traverse from the root
* Keep track of a potential successor:

  * If `p.data < curr.data`, then `curr` could be the successor → move left
  * Otherwise, move right
* The last recorded candidate is the answer

This approach avoids storing the full inorder traversal and runs efficiently.

---

## ✅ Java Solution

```java
import ds_v1.BinaryTree.TreeNode;

public class Solution {
    public static TreeNode<Integer> inorderSuccessor(
            TreeNode<Integer> root,
            TreeNode<Integer> p
    ) {
        TreeNode<Integer> successor = null;
        TreeNode<Integer> curr = root;

        while (curr != null) {
            if (p.data < curr.data) {
                successor = curr;      // possible successor
                curr = curr.left;
            } else {
                curr = curr.right;
            }
        }

        return successor;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(h)`
  *(h = height of the tree, worst-case O(n))*
* **Space Complexity:** `O(1)`