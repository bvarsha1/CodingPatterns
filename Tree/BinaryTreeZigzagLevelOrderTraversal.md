## Binary Tree Zigzag Level Order Traversal

#### Statement

Given a binary tree, return its zigzag level order traversal. The zigzag level order traversal corresponds to traversing nodes from left to right for one level, and then right to left for the next level, and so on, reversing direction after every level.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range `0` to `500`
* `−100 ≤ node.data ≤ 100`

---

## 🎯 Intuition

This problem is a **variation of level order traversal (BFS)** with one twist:

👉 **The direction of traversal alternates at each level**.

* Level 0 → left to right
* Level 1 → right to left
* Level 2 → left to right
* and so on…

### Key observations

* Level order traversal naturally groups nodes **by depth**
* We only need to **reverse the order** of values at every alternate level
* The tree structure itself does **not change**

### Strategy (BFS with Direction Toggle)

1. Use a **queue** to process nodes level by level
2. Maintain a boolean flag `leftToRight`
3. For each level:

   * Traverse all nodes in the queue
   * Store values in a list
   * If direction is right-to-left, reverse the list
4. Append the level result to the final answer
5. Flip the direction flag

---

## ✅ Java Solution (BFS – Level Order)

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
  public static List<List<Integer>> zigzagLevelOrder(TreeNode<Integer> root) {
    List<List<Integer>> ans = new ArrayList<>();
    if (root == null) return ans;

    Queue<TreeNode<Integer>> q = new LinkedList<>();
    q.offer(root);

    boolean leftToRight = true;

    while (!q.isEmpty()) {
      int size = q.size();
      List<Integer> level = new ArrayList<>();

      for (int i = 0; i < size; i++) {
        TreeNode<Integer> node = q.poll();
        level.add(node.data);

        if (node.left != null) q.offer(node.left);
        if (node.right != null) q.offer(node.right);
      }

      if (!leftToRight) {
        Collections.reverse(level);
      }

      ans.add(level);
      leftToRight = !leftToRight;
    }

    return ans;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Every node is visited once

* **Space Complexity:** `O(n)`
  * Queue + output storage