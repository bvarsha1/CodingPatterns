## Kth Smallest Element in a BST

#### Statement

Given the root node of a binary search tree and an integer value `k`, return the
kᵗʰ smallest value in the tree.

---

#### ✔️ Constraints

* The number of nodes in the tree is `n`
* `1 ≤ k ≤ n ≤ 500`
* `0 ≤ Node.data ≤ 10⁴`

---

## 🎯 Intuition

A **Binary Search Tree (BST)** has a very useful property:

> **Inorder traversal of a BST gives nodes in sorted (ascending) order.**

So if we perform an **inorder traversal**:

```
left → root → right
```

we will visit nodes from **smallest to largest**.

### Key idea

* Traverse the tree using **inorder DFS**
* Keep a counter of how many nodes we’ve visited
* When the counter reaches `k`, that node’s value is the answer

This avoids sorting or storing all values separately.

---

## ✅ Java Solution (Inorder DFS)

```java
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
    private static int count;
    private static int result;

    public static int kthSmallest(TreeNode<Integer> root, int k) {
        count = 0;
        result = -1;
        inorder(root, k);
        return result;
    }

    private static void inorder(TreeNode<Integer> node, int k) {
        if (node == null) return;

        // visit left subtree
        inorder(node.left, k);

        // process current node
        count++;
        if (count == k) {
            result = node.data;
            return;
        }

        // visit right subtree
        inorder(node.right, k);
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`

  * In worst case, we may visit all nodes
* **Space Complexity:** `O(h)`

  * Recursion stack, where `h` is the height of the tree
