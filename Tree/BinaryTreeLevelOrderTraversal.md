## Binary Tree Level Order Traversal

#### Statement

Given the root of a binary tree, display the values of its nodes while performing a level order traversal. Return the node values for all levels in a string separated by the character `:`. If the tree is empty, i.e., the number of nodes is `0`, then return `"None"` as the output.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range `[0, 500]`
* `−10³ ≤ Node.data ≤ 10³`

---

## 🎯 Intuition

**Level order traversal** means visiting the tree **level by level**, from top to bottom and from left to right.

The natural data structure for this traversal is a **queue**, because:

* Nodes are processed in the **order they are discovered**
* Children of the current level are enqueued for the next level

### Key requirements of this problem

1. Nodes must be grouped **by level**
2. Values from each level must be:

   * Printed in order
   * Joined together
3. Levels must be separated using the `:` character
4. If the tree is empty → return `"None"`

---

## 🧠 Strategy (BFS – Queue Based)

1. If `root == null`, return `"None"`
2. Initialize a queue and push the root
3. While the queue is not empty:

   * Record the **size of the queue** → number of nodes at this level
   * Process exactly `size` nodes
   * Enqueue their left and right children
   * Collect values of the current level
4. Join each level with `:` to form the final string

---

## ✅ Java Solution (BFS – Level Order)

```java
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

import java.util.*;
import ds_v1.BinaryTree.TreeNode;

public class Solution{
  public static String levelOrderTraversal(TreeNode<Integer> root) {
    String ans = "";
    if(root == null) return "None";
    
    Queue<TreeNode> q = new ArrayDeque<>();
    q.offer(root);
    
    while(!q.isEmpty()) {
      int size = q.size();
      if(q.peek() != root) ans += " : ";
      for(int i = 0; i < size; i++) {
        if(i != 0) ans += ", ";
        TreeNode tn = q.poll();
        if(tn.left != null) q.offer(tn.left);
        if(tn.right != null) q.offer(tn.right);
        ans += tn.data;
      }
    }
    
    return ans;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is visited once

* **Space Complexity:** `O(n)`
  * Queue can hold up to one full level of nodes