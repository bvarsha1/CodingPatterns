## Special BST! (Inorder Successor using Parent Pointer)

#### Statement

You are given a **special Binary Search Tree** in which every node contains a pointer to its **parent**.

You are given **only a target node**, not the root of the tree.
Your task is to find the **inorder successor** of the given node.

The node structure is as follows:

```java
class Node {
  int key;
  Node left;
  Node right;
  Node parent;
}
```

**Inorder Successor Definition**
The inorder successor of a node is the node with the **smallest key greater than the given node’s key** in the BST.

Return `null` if the inorder successor does not exist.

---

#### Constraints

* The tree contains nodes in the range
  [
  1 \le n \le 500
  ]
* [
  -10^4 \le Node.key \le 10^4
  ]
* All keys are **unique**
* The given node **exists** in the tree
* **Root is NOT provided**

---

## 🎯 Intuition

There are **two cases**:

### Case 1: Node has a right subtree

👉 The successor is the **leftmost node in the right subtree**

### Case 2: Node does NOT have a right subtree

👉 Move **up using parent pointers** until you find a node that is a **left child** of its parent
👉 That parent is the inorder successor

If you reach the top without finding such a node → successor does not exist

---

## Java Solution (Efficient – O(h) time, O(1) space)

```java
public class Solution {

  public static Node inorderSuccessor(Node p) {
    if (p == null) return null;

    // Case 1: Right subtree exists
    if (p.right != null) {
      return leftMost(p.right);
    }

    // Case 2: No right subtree
    Node curr = p;
    Node parent = p.parent;

    while (parent != null && parent.right == curr) {
      curr = parent;
      parent = parent.parent;
    }

    return parent; // may be null
  }

  private static Node leftMost(Node node) {
    while (node.left != null) {
      node = node.left;
    }
    return node;
  }
}
```

#### Complexity Analysis

* **Time Complexity:** `O(h)` where `h` is tree height
* **Space Complexity:** `O(1)` (no recursion, no stack)
