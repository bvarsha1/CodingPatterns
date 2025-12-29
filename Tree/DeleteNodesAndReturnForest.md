## Delete Nodes And Return Forest

#### Statement

Given the root of a binary tree where each node has a unique value, your task is to delete all nodes with values specified in the deleteNodes list. After performing the deletions, the tree will split into a forest—a collection of disjoint trees. Return the roots of the remaining trees in the forest in any order.

---

#### ✔️ Constraints

* `0 ≤ nodes ≤ 100`
* `1 ≤ nodes.value ≤ 1000`
* `0 ≤ deleteNodes.length ≤ 100`
* `1 ≤ deleteNodes[i] ≤ 1000`
* All node values are unique
* All values in `deleteNodes` are unique

---

## 🎯 Intuition

When a node is **deleted**, two things happen:

1. Its **children (if any)** become **new roots** of the forest
2. The deleted node itself is **removed from its parent**

So the problem becomes a **tree pruning** task.

### Key observations

* We must traverse **every node once**
* Whether a node stays in the forest depends on:

  * Is it marked for deletion?
  * Is its parent deleted?

### Strategy (DFS – Postorder)

We use **postorder DFS** so children are processed **before** the parent.

For each node:

1. Recursively process left and right subtrees
2. Check if the current node needs to be deleted
3. If **deleted**:
   * Its children (if not null) become new roots → add to result
   * Return `null` to the parent
4. If **not deleted**:
   * Return the node itself

Finally:
* If the **original root is not deleted**, include it in the forest

---

## ✅ Java Solution (DFS – Postorder)

```java
import ds_v1.BinaryTree.TreeNode;
import java.util.*;

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
  public static List<TreeNode<Integer>> returnForest(TreeNode<Integer> root, List<Integer> deleteNodes) {
    List<TreeNode<Integer>> forest = new ArrayList<>();
    Set<Integer> toDelete = new HashSet<>(deleteNodes);
    if(!toDelete.contains(root.data)) forest.add(root);
    
    dfs(root, forest, toDelete);

    return forest;
  }
  
  public static TreeNode<Integer> dfs(
    TreeNode<Integer> node,
    List<TreeNode<Integer>> forest,
    Set<Integer> toDelete
  ) {
    if(node == null) return null;
    
    node.left = dfs(node.left, forest, toDelete);
    node.right = dfs(node.right, forest, toDelete);
    
    if(toDelete.contains(node.data)) {
      if(node.left != null) forest.add(node.left);
      if(node.right != null) forest.add(node.right);
      return null;
    }
    
    return node;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is visited exactly once

* **Space Complexity:** `O(n)`
  * HashSet for deleted nodes
  * Recursion stack in worst case (skewed tree)
