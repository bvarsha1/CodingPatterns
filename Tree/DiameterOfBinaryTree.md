## Diameter of Binary Tree

#### Statement

Given a binary tree, you need to compute the length of the tree’s diameter. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

> **Note:** The length of the path between two nodes is represented by the number of edges between them.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range `[1, 500]`
* `−100 ≤ Node.value ≤ 100`

---

## 🎯 Intuition

The **diameter of a binary tree** can be understood as:

> The maximum number of edges between **any two nodes** in the tree.

For any node, the longest path that **passes through that node** is:

```
(height of left subtree) + (height of right subtree)
```

However, the overall diameter might also lie **entirely within the left subtree** or **entirely within the right subtree**.

So for each node, we need to compute **two things at once**:

1. **Height** of the subtree rooted at that node
2. **Diameter** of the subtree rooted at that node

To do this efficiently in one DFS traversal, the helper function returns an array:

```
[ height, diameter ]
```

---

## 🧠 How the Logic Works

For a given node:

* Recursively get:

  * `left = [leftHeight, leftDiameter]`
  * `right = [rightHeight, rightDiameter]`

Then compute:

* **Diameter passing through current node**

  ```
  D1 = leftHeight + rightHeight
  ```

* **Best diameter so far in subtrees**

  ```
  D2 = max(leftDiameter, rightDiameter)
  ```

* **Current subtree diameter**

  ```
  max(D1, D2)
  ```

* **Current subtree height**

  ```
  1 + max(leftHeight, rightHeight)
  ```

---

## ✅ Java Solution

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
    public static int diameterOfBinaryTree(TreeNode<Integer> root) {
        // diameter is stored at index 1
        return diameter(root)[1];
    }

    // returns int[] {height, diameter}
    public static int[] diameter(TreeNode<Integer> root) {
        if (root == null) {
            return new int[] {0, 0};
        }

        int[] left = diameter(root.left);
        int[] right = diameter(root.right);

        int height = 1 + Math.max(left[0], right[0]);

        int diameterThroughRoot = left[0] + right[0];
        int bestSubtreeDiameter = Math.max(left[1], right[1]);

        int diameter = Math.max(diameterThroughRoot, bestSubtreeDiameter);

        return new int[] { height, diameter };
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  (each node is visited once)

* **Space Complexity:** `O(h)`
  where `h` is the height of the tree (recursion stack)
