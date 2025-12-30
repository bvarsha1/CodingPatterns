## Two Sum IV – Input Is a BST

#### Statement

Given the root of a binary search tree and an integer k, determine whether there are two elements in the BST whose sum equals k. Return TRUE if such elements exist or FALSE otherwise.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range
  `[1, 10³]`
* `−10³ ≤ Node.data ≤ 10³`
* `root` is guaranteed to be a valid binary search tree
* `−10⁴ ≤ k ≤ 10⁴`

---

## 🎯 Intuition

The problem asks whether **any two distinct nodes** in the tree sum up to `k`.

Instead of relying on BST properties (like inorder traversal + two pointers), we can think of this as a classic **Two Sum** problem on a tree:

> As we traverse the tree, for each node with value `x`,
> check if we have already seen a value `k - x`.

### Why this works

* If at any point we’ve already seen `k - curr.data`, then:

  ```
  curr.data + (k - curr.data) = k
  ```
* We only need to traverse each node once.
* A `HashSet` allows `O(1)` lookup time.

### Traversal choice

We use **BFS (Level Order Traversal)**:

* Ensures all nodes are visited
* Simple to implement with a queue
* Does not rely on recursion (safe for deeper trees)

---

## ✅ Java Solution (BFS + HashSet)

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

public class Solution{
  public static boolean findTarget(TreeNode<Integer> root, int k) {
    if(root == null) return false;

    HashSet<Integer> seen = new HashSet<>();
    Queue<TreeNode<Integer>> q = new ArrayDeque<>();
    q.offer(root);
    
    while(!q.isEmpty()) {
      TreeNode<Integer> curr = q.poll();
      
      // check if complement exists
      if(seen.contains(k - curr.data)) return true;
      
      // add children to queue
      if(curr.left != null) q.offer(curr.left);
      if(curr.right != null) q.offer(curr.right);
      
      // mark current value as seen
      seen.add(curr.data);
    }
    
    return false;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is visited once

* **Space Complexity:** `O(n)`
  * HashSet + queue can store up to `n` nodes
