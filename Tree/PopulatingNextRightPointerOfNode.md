## Populating Next Right Pointers in Each Node

#### Statement

Given a perfect binary tree, where each node contains an additional pointer called `next`. This pointer is initially set to `NULL` for all nodes. Your task is to connect all nodes of the same hierarchical level by setting the `next` pointer to its immediate right node.

The `next` pointer of the rightmost node at each level is set to `NULL`.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range `[0, 500]`
* `−1000 ≤ Node.data ≤ 1000`

---

## 🎯 Intuition

Since we need to connect nodes **level by level**, this problem naturally fits a **Breadth First Search (BFS)** approach.

Key ideas:

* Nodes at the **same level** should be connected from **left → right**
* The **last node** in each level should point to `NULL`
* BFS processes the tree **level by level**, which makes linking straightforward

---

## 🧠 Strategy (BFS – Level Order Traversal)

1. Use a **queue** to perform level order traversal
2. For each level:

   * Iterate through all nodes at that level
   * Connect the previous node’s `next` to the current node
3. After finishing a level:

   * Ensure the last node’s `next` is set to `null`

---

## ✅ Java Solution (BFS)

```java
import java.util.*;

// Definition for a Node with next pointer
// class Node {
//     int data;
//     Node left;
//     Node right;
//     Node next;
//
//     Node(int data) {
//         this.data = data;
//         this.left = null;
//         this.right = null;
//         this.next = null;
//     }
// }

public class Solution {
  public static Node connect(Node root) {
    if (root == null) return null;

    Queue<Node> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
      int size = queue.size();
      Node prev = null;

      for (int i = 0; i < size; i++) {
        Node curr = queue.poll();

        if (prev != null) {
          prev.next = curr;
        }
        prev = curr;

        if (curr.left != null) queue.offer(curr.left);
        if (curr.right != null) queue.offer(curr.right);
      }

      // last node of the level
      if (prev != null) {
        prev.next = null;
      }
    }

    return root;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is visited once

* **Space Complexity:** `O(n)`
  * Queue stores nodes level by level