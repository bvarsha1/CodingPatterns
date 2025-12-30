
## Vertical Order Traversal of a Binary Tree

#### Statement

Find the vertical order traversal of a binary tree when the root of the binary tree is given. In other words, return the values of the nodes from top to bottom in each column, column by column from left to right. If there is more than one node in the same column and row, return the values from left to right.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range `[1, 500]`
* `0 ≤ Node.data ≤ 1000`

---

## 🎯 Intuition

In **vertical order traversal**, every node is assigned a **column index**:

* Root → column `0`
* Left child → column `col - 1`
* Right child → column `col + 1`

We must output nodes:

1. **Column by column** (from leftmost to rightmost)
2. **Top to bottom** within each column
3. **Left to right** if nodes share the same row and column

### Why BFS?

Using **Breadth First Search (BFS)** is ideal because:

* BFS naturally processes nodes **level by level** (top to bottom)
* When multiple nodes fall in the same column & row, BFS preserves **left-to-right order**
* We can track each node’s column index while traversing

---

## 🧠 Strategy (BFS + Column Mapping)

1. Use a **queue** to perform BFS

   * Store `(node, column)` pairs
2. Use a **map**:

   * `column → list of node values`
3. Track:

   * `minCol` → leftmost column
   * `maxCol` → rightmost column
4. After traversal:

   * Iterate from `minCol` to `maxCol`
   * Collect values column by column

---

## ✅ Java Solution (BFS Approach)

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

  public static class Pair {
    TreeNode<Integer> node;
    int col;
    public Pair(TreeNode<Integer> node, int col) {
      this.node = node;
      this.col = col;
    }
  }
  
  public static List<List<Integer>> verticalOrder(TreeNode<Integer> root) {
    List<List<Integer>> ans = new ArrayList<>();
    if(root == null) return ans;
    
    Map<Integer, List<Integer>> colMap = new HashMap<>();
    Queue<Pair> q = new LinkedList<>();
    
    int minCol = 0, maxCol = 0;
    q.offer(new Pair(root, 0));
    
    while(!q.isEmpty()) {
      Pair curr = q.poll();
      TreeNode<Integer> currNode = curr.node;
      int col = curr.col;
      
      colMap.putIfAbsent(col, new ArrayList<>());
      colMap.get(col).add(currNode.data);
      
      minCol = Math.min(minCol, col);
      maxCol = Math.max(maxCol, col);
      
      if(currNode.left != null) q.offer(new Pair(currNode.left, col - 1));
      if(currNode.right != null) q.offer(new Pair(currNode.right, col + 1));
    }
    
    for(int c = minCol; c <= maxCol; c++)
      ans.add(colMap.get(c));
      
    return ans;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is visited exactly once

* **Space Complexity:** `O(n)`
  * Queue + HashMap storage
