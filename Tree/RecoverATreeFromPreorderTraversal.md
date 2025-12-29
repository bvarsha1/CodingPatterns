## Recover a Tree From Preorder Traversal

#### Statement

We perform a preorder depth-first traversal on a binary tree starting from its root.

- For each node, we first write D dashes, where D is the depth of the node in the tree, followed by the node’s integer value.

- The root node has depth 0.

- If a node is at depth D, its children (if any) will appear at depth D + 1.

- If a node has only one child, it will always be the left child.

You are given the string representation of this traversal. Your task is to reconstruct the original binary tree and return its root.

---

#### ✔️ Constraints

* The number of nodes in the original binary tree lies within the range `[1, 500]`
* `1 ≤ node.data ≤ 10³`

---

## 🎯 Intuition

This problem is about **parsing a preorder traversal string** where:

* **Depth** is encoded using `-` (dashes)
* **Node value** comes immediately after the dashes
* The traversal order is **Root → Left → Right**

### Key observations

1. Preorder traversal means:

   * We always process a node **before** its children
2. The number of dashes (`-`) tells us **exactly where the node belongs**
3. If a node has only one child, it is **guaranteed to be the left child**

### Core idea (DFS Parsing)

We parse the string **from left to right**, using a **global index**.

For each recursive DFS call:

1. Count how many dashes appear → this gives the **current depth**
2. If the depth doesn’t match what we expect, this node does **not belong here**
3. Parse the number to create the node
4. Recursively build:

   * left subtree (depth + 1)
   * right subtree (depth + 1)

This mirrors how preorder traversal was originally created.

---

## ✅ Java Solution (DFS Parsing)

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
    static int idx = 0;

    public static TreeNode<Integer> recoverFromPreorder(String traversal) {
        idx = 0;
        return dfs(traversal, 0);
    }

    private static TreeNode<Integer> dfs(String s, int depth) {
        int n = s.length();
        int temp = idx;

        // count dashes
        int dashCount = 0;
        while (temp < n && s.charAt(temp) == '-') {
            dashCount++;
            temp++;
        }

        // depth mismatch → node does not belong here
        if (dashCount != depth) {
            return null;
        }

        // move index past dashes
        idx = temp;

        // parse number
        int val = 0;
        while (idx < n && Character.isDigit(s.charAt(idx))) {
            val = val * 10 + (s.charAt(idx) - '0');
            idx++;
        }

        TreeNode<Integer> node = new TreeNode<>(val);

        // build left and right subtrees
        node.left = dfs(s, depth + 1);
        node.right = dfs(s, depth + 1);

        return node;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`

  * Each character is processed once
* **Space Complexity:** `O(n)`

  * Recursion stack in worst case (skewed tree)
