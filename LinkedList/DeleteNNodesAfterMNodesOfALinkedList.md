## Delete N Nodes After M Nodes of a Linked List

#### Statement

Given the head of a linked list and two integers, `m` and `n`, remove some specific nodes from the list and return the head of the modified, linked list. The list should be traversed, and nodes removed as follows:

* Start with the head node and set it as the current node.
* Traverse the next `m` nodes from the current node without deleting them.
* Traverse the next `n` nodes and delete them.
* Repeat steps 2 and 3 above until the end of the linked list is reached.

---

#### ✔️ Constraints

* `1 ≤ Node ≤ 10³`, where `Node` is the number of nodes in the list.
* `1 ≤ Node.val ≤ 10³`
* `1 ≤ m, n ≤ 500`

---

## 🎯 Intuition

We traverse the linked list in **cycles of keeping and deleting nodes**:

1. Move forward `m` nodes — these nodes are kept as-is.
2. From the next position, skip (delete) the next `n` nodes by adjusting pointers.
3. Repeat this process until we reach the end of the list.

Since we only manipulate pointers and do not use extra data structures, the solution runs efficiently with constant extra space.

---

## ✅ Java Solution

```java
// Definition for a Linked List node

// class ListNode {
//     int val;
//     ListNode next;

//     // Constructor
//     public ListNode(int val) {
//         this.val = val;
//         this.next = null;
//     }
// }
import ds_v1.LinkedList.ListNode;

public class Solution {
    public static ListNode deleteNodes(ListNode head, int m, int n) {
        ListNode curr = head, lastOfM = null;
        while(curr != null) {
            int i = m;
            while(curr != null && i > 0) {
                lastOfM = curr;
                curr = curr.next;
                i--;
            }
            
            int j = n;
            while(curr != null && j > 0) {
                curr = curr.next;
                j--;
            }
            
            if(lastOfM != null) {
                lastOfM.next = curr;
            }
        }
        
        return head;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
