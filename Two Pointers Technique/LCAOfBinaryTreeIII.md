## Lowest Common Ancestor of a Binary Tree III

#### 🧾 Problem Statement

You are given two nodes `p` and `q` of a binary tree.
Each node has a pointer to its **parent** (but the root is unknown).

Return their **Lowest Common Ancestor (LCA)** —
the lowest node in the tree that has both `p` and `q` as descendants
(a node is considered a descendant of itself).

---

#### ✔️ Constraints

* `2 ≤ number of nodes ≤ 500`
* `-10⁴ ≤ Node.data ≤ 10⁴`
* All values are **unique**
* Both `p` and `q` exist in the tree
* `p != q`

---

## 💡 Intuition

Use **two pointers**, `a` and `b`:
- Start `a` at `p`, `b` at `q`
- Move each pointer **upward** by jumping to their parent
- If a pointer reaches `null`, teleport it to the **other node’s starting position**

Eventually both pointers will traverse equal effective path length and converge:
- If `p` and `q` meet earlier → that is LCA
- If not, both end together at root → the LCA is root

This guarantees meeting at **exactly** the LCA without extra memory.

---

## ✅ Java Solution

```java
import java.util.*;
// Definiton of a binary tree node class
// class EduTreeNode {
//     int data;
//     EduTreeNode left;
//     EduTreeNode right;
//     EduTreeNode parent;

//     EduTreeNode(int value) {
//         this.data = value;
//         this.left = null;
//         this.right = null;
//         this.parent = null;
//     }
// }

public class Solution {
    public EduTreeNode LowestCommonAncestor(EduTreeNode p, EduTreeNode q) {
      EduTreeNode a = p, b = q;
      
      while(a != b) {
        a = (a == null) ? q : a.parent;
        b = (b == null) ? p : b.parent;
      }
      
      return a;
    }
}
```
- **Time Complexity: O(h)**
  - `h` is the height of the binary tree
- **Space Complexity: O(1)**