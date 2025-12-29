## Shortest Tree Path

#### Statement

Given a **Binary Search Tree (BST)** and the values of two nodes that lie inside the tree, find the **shortest path length** (number of edges) between the two nodes.

```
         10
       /     \
      4      15
    /   \   /   \
   2     5 13   22
 /           \
1            14
```

**Examples**

* Shortest Distance between `(1, 4)` is `2`
* Shortest Distance between `(2, 13)` is `4`
* Shortest Distance between `(5, 14)` is `5`

---


## Intuition

Because this is a **BST**:

* If both values are smaller than current node → go left
* If both values are larger → go right
* Otherwise, current node is the **LCA**

### Line-by-Line Explanation

#### `shortestDistance`

* Finds LCA
* Computes distance from LCA → `p`
* Computes distance from LCA → `q`
* Returns sum

#### `findLCA`

* Uses BST ordering to move left/right
* Stops when paths to `p` and `q` diverge
* That node is the **lowest common ancestor**

#### `distanceFromNode`

* Walks down the BST
* Counts number of edges until target is found

---

#### Solution (Efficient – O(h) time)

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
  public static int count;
  public static int kthSmallestElement(TreeNode<Integer> root, int k) {
    count = 0;
    return dfs(root, k);
  }
  
  public static int dfs(TreeNode<Integer> root, int k) {
    if(root == null) return 0;
    
    int left = dfs(root.left, k);
    if(left != 0) return left;
    count++;
    if(count == k) return root.data;
    int right = dfs(root.right, k);
    return right;
  }
}
```

#### Complexity Analysis

* **Time Complexity:** `O(h)` where `h` is height of BST
* **Space Complexity:** `O(1)` (iterative, no recursion)
