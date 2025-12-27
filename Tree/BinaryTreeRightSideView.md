## Binary Tree Right Side View

#### Statement

You are given a root of a binary tree that has n number of nodes. You have to return the right-side view in the form of a list.

A right-side view of a binary tree is the data of the nodes that are visible when the tree is viewed from the right side.

---

#### ✔️ Constraints

* `0 ≤ n ≤ 100`
* `−100 ≤ Node.data ≤ 100`

---

## 🎯 Intuition

When viewing a binary tree from the **right side**, for each depth (or level) of the tree, **only one node is visible**:

👉 the **rightmost node at that level**.

There are two common ways to solve this:

* **Level Order Traversal (BFS)** – take the last node at each level
* **Depth First Search (DFS)** – prioritize the **right subtree first**

Here we use **DFS**, because it allows us to:

* Traverse the tree **top-down**
* Visit the **right child before the left**
* Capture the **first node encountered at each level**

Key idea:

* Maintain a list `ans`
* The **first time** we reach a level, that node is the rightmost one
* Since we traverse `right → left`, the first node seen at each level is correct

---

## ✅ Java Solution (DFS – Right First)

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
    public static List<Integer> rightSideView(TreeNode<Integer> root) {
        List<Integer> ans = new ArrayList<>();
        dfsHelper(root, ans, 0);
        return ans;
    }

    public static void dfsHelper(TreeNode<Integer> root, List<Integer> ans, int level) {
        // base case
        if (root == null) return;

        // first node reached at this level → rightmost
        if (ans.size() == level) {
            ans.add(root.data);
        }

        // visit right subtree first
        dfsHelper(root.right, ans, level + 1);
        dfsHelper(root.left, ans, level + 1);
    }
}
```

## ⏱ Complexity

* **Time Complexity:** `O(n)`
  (Each node is visited once)
* **Space Complexity:** `O(h)`
  (Recursion stack, where `h` is the height of the tree)