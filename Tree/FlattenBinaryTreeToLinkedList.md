## Flatten Binary Tree to Linked List

#### Statement

Given the root of a binary tree, the task is to flatten the tree into a linked list using the same `TreeNode` class. The left child pointer of each node in the linked list should always be `NULL`, and the right child pointer should point to the next node in the linked list. The nodes in the linked list should be in the same order as that of the **preorder traversal** of the given binary tree.

---

#### ✔️ Constraints

* `-100 ≤ Node.data ≤ 100`
* The tree contains nodes in the range `[1, 500]`

---

## 🎯 Intuition

The required linked list must follow **preorder traversal**:

```
Root → Left → Right
```

Key observations:

* We are **not allowed to create new nodes**
* We must **rearrange pointers in-place**
* After flattening:

  * `node.left` must always be `null`
  * `node.right` points to the next preorder node

### Core idea (in-place):

For every node:

1. If it has a **left subtree**:

   * Find the **rightmost node** of the left subtree
   * Attach the original `right` subtree to this rightmost node
   * Move the left subtree to the right
   * Set `left = null`
2. Move to `node.right` and repeat

This mimics preorder traversal while rewiring pointers.

---

## ✅ Java Solution (Iterative, In-Place)

```java
// Definition for a binary tree node
// class TreeNode {
//     int data;
//     TreeNode left;
//     TreeNode right;
//     TreeNode(int x) { data = x; }
// }

public class Solution {
    public void flatten(TreeNode root) {
        TreeNode curr = root;

        while (curr != null) {
            if (curr.left != null) {
                // find rightmost node of left subtree
                TreeNode prev = curr.left;
                while (prev.right != null) {
                    prev = prev.right;
                }

                // rewire connections
                prev.right = curr.right;
                curr.right = curr.left;
                curr.left = null;
            }
            curr = curr.right;
        }
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)` (in-place, no recursion or stack)

---

## ✅ Java Solution (Recursive, In-Place)

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
	public static TreeNode<Integer> flattenTree(TreeNode<Integer> root) {
    dfs(root);
    return root;
	}
	
	public static TreeNode<Integer> dfs(TreeNode<Integer> root) {
	  if(root == null) return null;
	  
	  TreeNode<Integer> leftTail = dfs(root.left);
	  TreeNode<Integer> rightTail = dfs(root.right);
	  
	  if(leftTail != null) {
	    leftTail.right = root.right;
	    root.right = root.left;
	    root.left = null;
	  }
	  
    if(rightTail != null) return rightTail;
    else if(leftTail != null) return leftTail;
    else return root;
	}
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)` (in-place, no recursion or stack)