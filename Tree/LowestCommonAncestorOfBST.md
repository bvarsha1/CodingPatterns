## Lowest Common Ancestor of a Binary Search Tree

#### Statement

Given the root node of a **binary search tree (BST)** and two nodes `p` and `q`, your task is to find their **lowest common ancestor (LCA)**.

> The lowest common ancestor of two nodes `p` and `q` is defined as the **lowest node in the BST that has both `p` and `q` as descendants**.
> A node can be a descendant of itself.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range `[1, 500]`
* `−10^4 ≤ Node.data ≤ 10^4`
* All node values are **unique**
* Both `p` and `q` exist in the BST
* `p != q`

---

## 🎯 Intuition

This problem becomes **simpler than the general Binary Tree LCA** because of the **BST property**:

* All values in the **left subtree** are smaller
* All values in the **right subtree** are larger

So at any node:

* If **both `p` and `q` are smaller**, LCA lies in the **left subtree**
* If **both `p` and `q` are larger**, LCA lies in the **right subtree**
* Otherwise:

  * The current node **splits** the paths to `p` and `q`
  * This node is the **LCA**

This allows us to solve the problem in **O(height of tree)** time without extra space.

---

## ✅ Java Solution (BST Optimized)

```java
import ds_v1.BinaryTree.TreeNode;

public class Solution {
    public static TreeNode<Integer> lowestCommonAncestor(
            TreeNode<Integer> root,
            TreeNode<Integer> p,
            TreeNode<Integer> q
    ) {
        TreeNode<Integer> curr = root;

        while (curr != null) {
            // both nodes lie in left subtree
            if (p.data < curr.data && q.data < curr.data) {
                curr = curr.left;
            }
            // both nodes lie in right subtree
            else if (p.data > curr.data && q.data > curr.data) {
                curr = curr.right;
            }
            // split happens here → this is LCA
            else {
                return curr;
            }
        }
        return null;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(h)`
  where `h` is the height of the BST
* **Space Complexity:** `O(1)` (iterative approach)