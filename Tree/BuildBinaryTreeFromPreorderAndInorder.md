## Build Binary Tree from Preorder and Inorder Traversal

#### Statement

Create a binary tree from two integer arrays, `pOrder` and `iOrder`, where `pOrder` represents a preorder traversal of a binary tree, and `iOrder` represents an inorder traversal of the same tree.

---

#### ✔️ Constraints

* `1 ≤ pOrder.length, iOrder.length ≤ 1000`
* `iOrder.length == pOrder.length`
* `−1000 ≤ pOrder[i], iOrder[i] ≤ 1000`
* `pOrder` and `iOrder` consist of unique values
* Each value of `iOrder` also appears in `pOrder` and vice versa

---

## 🎯 Intuition

This problem relies on the **fundamental properties of tree traversals**:

### Key observations

* **Preorder traversal**:
  `Root → Left → Right`
  → The **first element** is always the root of the (sub)tree.

* **Inorder traversal**:
  `Left → Root → Right`
  → Elements **left of the root** belong to the left subtree,
  elements **right of the root** belong to the right subtree.

### Strategy

1. Use a pointer `preIdx` to track the current root in `pOrder`
2. Use a **HashMap** to store the index of each value in `iOrder` for O(1) lookup
3. Recursively:

   * Pick the current root from `pOrder`
   * Find its position in `iOrder`
   * Split the inorder range into left and right subtrees
4. Build the tree top-down using DFS

This guarantees that the tree structure matches both traversals.

---

## ✅ Java Solution

```java
import java.util.*;
import ds_v1.BinaryTree.TreeNode;

// Definition of a binary tree node class
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
    public static int preIdx;
    public static HashMap<Integer, Integer> inMap;

    public static TreeNode<Integer> buildTree(int[] pOrder, int[] iOrder) {
        preIdx = 0;
        inMap = new HashMap<>();

        // store inorder indices for fast lookup
        for (int i = 0; i < iOrder.length; i++) {
            inMap.put(iOrder[i], i);
        }

        return dfs(pOrder, 0, iOrder.length - 1);
    }

    public static TreeNode<Integer> dfs(int[] pOrder, int s, int e) {
        // base case
        if (s > e) return null;

        // create root using preorder
        TreeNode<Integer> root = new TreeNode<>(pOrder[preIdx++]);

        // split inorder into left and right subtrees
        int inIdx = inMap.get(root.data);

        root.left = dfs(pOrder, s, inIdx - 1);
        root.right = dfs(pOrder, inIdx + 1, e);

        return root;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  (each node is processed once)

* **Space Complexity:** `O(n)`
  (HashMap + recursion stack)
