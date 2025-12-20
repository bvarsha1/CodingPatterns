## Insert into a Sorted Circular Linked List

#### Statement

You’re given a reference to a node, `head`, in a circular linked list, where the values are sorted in non-decreasing order. The list is circular, so the last node points to the first node. However, the head can be any node in the list—it is not guaranteed to be the node with the smallest value.

Your task is to insert a new value, `insertVal`, into the list so that it remains sorted and circular after the insertion.

* You can choose any of the multiple valid spots where the value can be inserted while maintaining the sort order.

* If the list is empty (i.e., the given node is `NULL`), create a new circular list with a single node containing `insertVal`, and return that node.

* Otherwise, return the head node after insertion.

---

#### ✔️ Constraints

* The number of nodes in the list is in the range `[0, 10³]`
* `-10³ ≤ Node.val, insertVal ≤ 10³`

---

## 🎯 Intuition

Because the list is **circular** and **sorted**, but the `head` can be **any node**, we cannot assume we start from the smallest value.

We traverse the list and look for one of the following valid insertion cases:

1. **Normal case**:
   `curr.val ≤ insertVal ≤ curr.next.val`
   → insert between `curr` and `curr.next`.

2. **Rotation point (max → min)**:
   When `curr.val > curr.next.val`, we are at the boundary between the largest and smallest values.
   If `insertVal` is:

   * greater than or equal to the maximum, or
   * less than or equal to the minimum
     → insert here.

3. **All values are the same** or no case matched after a full cycle:
   → insert anywhere.

If the list is empty, we simply create a node that points to itself.

---

## ✅ Java Solution

```java
import java.util.*;
// Definition for a Linked List node
// class Node {
//     int val;
//     Node next;

//     // Constructor
//     public Node(int val) {
//         this.val = val;
//         this.next = null;
//     }

//     public Node(int val, Node next) {
//         this.val = val;
//         this.next = next;
//     }
// }
public class Solution
{
    public Node insert(Node head, int insertVal) 
    {
        Node node = new Node(insertVal);
        // Case 1: empty list
        if(head == null) {
            node.next = node;
            return node;
        }
        
        Node curr = head;
        while(true) {
            // Case 2: normal insertion point
            if(curr.val <= insertVal && insertVal <= curr.next.val) break;
            
            // Case 3: rotation point (max -> min)
            if(curr.val > curr.next.val) {
                if(insertVal >= curr.val || insertVal <= curr.next.val) break;
            }
            
            curr = curr.next;
            // Case 4: completed a full circle
            if(curr == head) break;
        }
        
        node.next = curr.next;
        curr.next = node;
        
        return head;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
