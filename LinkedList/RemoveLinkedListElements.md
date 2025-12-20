## Remove Linked List Elements

#### Statement

You are given the head of a linked list and an integer `k`. Remove all nodes from the linked list where the node’s value equals `k`, and return the head of the updated list.

---

#### ✔️ Constraints

* The number of nodes in the linked list is in the range `[0, 10³]`
* `1 ≤ Node.val ≤ 50`
* `0 ≤ k ≤ 50`

---

## 🎯 Intuition

We need to remove **all nodes whose value equals `k`**, including the possibility that the **head itself** needs to be removed.

To handle this cleanly:

* Use a **dummy node** that points to the head
* Traverse the list with a pointer
* If the next node’s value equals `k`, skip it
* Otherwise, move forward

Using a dummy node avoids special-case handling for removing the head.

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
import java.util.*;
import ds_v1.LinkedList.ListNode;

public class Solution {
    public static ListNode removeElements(ListNode head, int k) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode curr = dummy;
        while(curr.next != null) {
            if(curr.next.val == k) {
                curr.next = curr.next.next;
            } else {
                curr = curr.next;
            }
        }
        return dummy.next;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)

You can send the **next problem** whenever you’re ready — I’ll keep this **exact Markdown format** unchanged.
