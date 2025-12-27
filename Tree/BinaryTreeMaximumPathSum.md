## Binary Tree Maximum Path Sum

#### Statement

Given the root of a binary tree, return the maximum sum of any non-empty path.

A path in a binary tree is defined as follows:

* A sequence of nodes in which each pair of adjacent nodes must have an edge connecting them.
* A node can only be included in a path once at most.
* Including the root in the path is not compulsory.
* You can calculate the path sum by adding up all node values in the path.

To solve this problem, calculate the maximum path sum given the root of a binary tree so that there won’t be any greater path than it in the tree.

---

#### ✔️ Constraints

* `1 ≤ Number of nodes in the tree ≤ 500`
* `−1000 ≤ Node.value ≤ 1000`

---

## 🎯 Intuition

This is a classic **tree DP** problem.

Key observations:

* A **path** can start and end at **any two nodes**
* At each node, we have two choices:

  1. Use the node as a **bridge** connecting left and right subtrees
  2. Extend a path **upwards** to its parent

So for every node, we compute:

1. **Max path sum starting at this node and going upward**

   * This can only take **one side** (left or right)
2. **Max path sum passing through this node**

   * This can take **both left and right**

We maintain a **global maximum** to track the best path found anywhere in the tree.

Important detail:

* If a subtree contributes a **negative sum**, we ignore it (treat it as `0`), because including it would reduce the total.

---

## ✅ Java Solution

```java
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

import java.util.*;
import ds_v1.BinaryTree.TreeNode;

public class Solution{
  public static int res = Integer.MIN_VALUE;
  public static int maxPathSum(TreeNode<Integer> root) {
    res = Integer.MIN_VALUE;
    maxContribution(root);
    return res;
  }
  
  public static int maxContribution(TreeNode<Integer> root) {
    if(root == null) return 0;
    
    int left = 0, right = 0;
    left = Math.max(0, maxContribution(root.left));
    right = Math.max(0, maxContribution(root.right));

    res = Math.max(res, root.data + left + right);
    return root.data + Math.max(left, right);
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  (Each node is visited once)

* **Space Complexity:** `O(h)`
  where `h` is the height of the tree (recursion stack)
