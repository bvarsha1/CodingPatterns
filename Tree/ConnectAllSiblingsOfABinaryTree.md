## Connect All Siblings of a Binary Tree

#### Statement

Given the root of a perfect binary tree, where each node is equipped with an additional pointer, `next`, connect all nodes from left to right. Do so in such a way that the `next` pointer of each node points to its immediate right sibling **except for the rightmost node**, which points to the **first node of the next level**.

The `next` pointer of the **last node of the binary tree** (i.e., the rightmost node of the last level) should be set to `NULL`.

---

#### ✔️ Constraints

* `0 ≤ number of nodes ≤ 500`
* `−1000 ≤ Node.data ≤ 1000`

---

## 🎯 Intuition

This problem is an extension of **level-order traversal (BFS)** with a twist:

* Instead of connecting nodes **within the same level only**
* We must connect **all nodes in BFS order**, level by level

### What does “connect all siblings” mean?

If we traverse the tree in **level order**, the nodes appear in this sequence:

```
Level 0:  1
Level 1:  2 → 3
Level 2:  4 → 5 → 6 → 7
```

The required `next` connections should look like:

```
1 → 2 → 3 → 4 → 5 → 6 → 7 → NULL
```

👉 So essentially:

* Perform **BFS**
* Maintain a pointer to the **previous node**
* Connect `prev.next = current`
* Update `prev`

---

## 🧠 Key Observations

* A **queue** naturally gives nodes in left-to-right, top-to-bottom order
* We do **not reset** pointers at level boundaries
* Only the **very last node** points to `NULL`

---

## ✅ Java Solution (BFS)

```java
import java.util.*;
import ds_v1.BinaryTree.TreeNode;

// Definition of a binary tree node with next pointer
// class TreeNode<T> {
//     T data;
//     TreeNode<T> left;
//     TreeNode<T> right;
//     TreeNode<T> next;
//
//     TreeNode(T data) {
//         this.data = data;
//         this.left = null;
//         this.right = null;
//         this.next = null;
//     }
// }

public class Solution {
  public static void connectAllSiblings(TreeNode<Integer> root) {
    if (root == null) return;

    Queue<TreeNode<Integer>> q = new ArrayDeque<>();
    q.offer(root);

    TreeNode<Integer> prev = null;

    while (!q.isEmpty()) {
      TreeNode<Integer> curr = q.poll();

      // connect previous node to current
      if (prev != null) {
        prev.next = curr;
      }

      prev = curr;

      if (curr.left != null) q.offer(curr.left);
      if (curr.right != null) q.offer(curr.right);
    }

    // last node automatically points to null
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is processed once

* **Space Complexity:** `O(n)`
  * Queue for BFS traversal