## Average of Levels in Binary Tree

#### Statement

You are given the root of a binary tree. Your task is to return the average value of the nodes on each level in the form of an array.

> **Note:** Only answers within `10^-5` of the actual answer will be accepted.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range
  `[1, 10⁴]`
* `−2³¹ ≤ Node.data ≤ 2³¹ − 1`

---

## 🎯 Intuition

This is a **level-wise aggregation** problem.

Key observations:

* A binary tree naturally splits into **levels** (depth 0, depth 1, depth 2, …)
* For each level, we need:

  * **Sum of all node values**
  * **Count of nodes**
  * Average = `sum / count`

The most natural way to process a tree **level by level** is **Breadth-First Search (BFS)** using a queue.

---

## 🧠 Strategy (BFS – Level Order Traversal)

1. Use a queue and start by inserting the `root`
2. While the queue is not empty:

   * Capture the current `size` of the queue → number of nodes at this level
   * Iterate `size` times:

     * Pop nodes
     * Add their values to a running `sum`
     * Push their children (if any) into the queue
   * Compute `average = sum / size`
   * Store it in the result list
3. Repeat for all levels

### Why use `Long` for sum?

* Node values can be as large as `±2³¹`
* Summing many integers may overflow `int`
* Using `Long` prevents overflow safely

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
    public static List<Double> averageOfLevels(TreeNode<Integer> root) {
        List<Double> ans = new ArrayList<>();
        if (root == null) return ans;

        Queue<TreeNode<Integer>> q = new ArrayDeque<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();
            long sum = 0L;

            for (int i = 0; i < size; i++) {
                TreeNode<Integer> curr = q.poll();
                sum += curr.data;

                if (curr.left != null) q.offer(curr.left);
                if (curr.right != null) q.offer(curr.right);
            }

            ans.add((double) sum / size);
        }

        return ans;
    }
}
```

#### ⏱ Complexity Analysis

* **Time Complexity:** `O(n)`
  * Each node is visited exactly once

* **Space Complexity:** `O(w)`
  * Queue stores at most `w` nodes, where `w` is the maximum width of the tree
  * Worst case: `O(n)`
