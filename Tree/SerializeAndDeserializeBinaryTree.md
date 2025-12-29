## Serialize and Deserialize Binary Tree

#### Statement

Serialize a given binary tree to a file and deserialize it back to a tree. Make sure that the original and the deserialized trees are identical.

Serialize: Write the tree to a file.
Deserialize: Read from a file and reconstruct the tree in memory.

Serialize the tree into a list of integers, and then, deserialize it back from the list to a tree. For simplicity’s sake, there’s no need to write the list to the files.

---

#### ✔️ Constraints

* The number of nodes in the tree is in the range
  [
  0 , 500
  ]
* `−1000 ≤ Node.value ≤ 1000`

---

## 🎯 Intuition

To **serialize** and **deserialize** a binary tree reliably, we must preserve both:

1. **Node values**
2. **Tree structure**

A common and effective approach is:

### 👉 Preorder Traversal + Null Markers

**Why preorder?**

* Root is processed first
* Makes reconstruction straightforward during deserialization

**Why store `"null"`?**

* Without null markers, we cannot distinguish between different tree shapes
* `"null"` explicitly captures missing children

### Serialization idea

* Traverse the tree in **preorder**
* For every node:

  * Add its value to the list
  * If a node is `null`, add `"null"`

### Deserialization idea

* Read values in the same order
* Use an iterator to consume elements **exactly once**
* Rebuild the tree recursively:

  * `"null"` → return `null`
  * Otherwise → create node, then build left and right subtrees

This guarantees the deserialized tree is **identical** to the original.

---

## ✅ Java Solution (Preorder DFS)

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

public class SerializeDeserialize {

    // Serialize the tree into a list
    public static List<String> serialize(TreeNode<Integer> root) {
        List<String> res = new LinkedList<>();
        dfsSerialize(root, res);
        return res;
    }

    private static void dfsSerialize(TreeNode<Integer> root, List<String> res) {
        // base case
        if (root == null) {
            res.add("null");
            return;
        }

        // preorder: root → left → right
        res.add(String.valueOf(root.data));
        dfsSerialize(root.left, res);
        dfsSerialize(root.right, res);
    }

    // Deserialize the list back into a tree
    public static TreeNode<Integer> deserialize(List<String> stream) {
        Iterator<String> it = stream.iterator();
        return dfsDeserialize(it);
    }

    private static TreeNode<Integer> dfsDeserialize(Iterator<String> it) {
        if (!it.hasNext()) return null;

        String val = it.next();
        if (val.equals("null")) {
            return null;
        }

        TreeNode<Integer> node = new TreeNode<>(Integer.parseInt(val));
        node.left = dfsDeserialize(it);
        node.right = dfsDeserialize(it);

        return node;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each node is visited once during serialization and deserialization
* **Space Complexity:** `O(n)`
  * Serialized list + recursion stack

