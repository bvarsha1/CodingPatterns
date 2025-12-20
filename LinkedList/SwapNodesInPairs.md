## Swap Nodes in Pairs

#### Statement

Given a singly linked list, swap every two adjacent nodes of the linked list. After the swap, return the head of the linked list.

> **Note:** Solve the problem without modifying the values in the list’s nodes. In other words, only the nodes themselves can be changed.

---

#### ✔️ Constraints

* The number of nodes in the list is in the range `[0, 100]`
* `0 ≤ Node.value ≤ 100`

---

## 🎯 Intuition

We need to swap **pairs of adjacent nodes** by changing links, not values.

Key idea:

* Use a **dummy node** pointing to the head to simplify handling the first pair
* Always process two nodes at a time:

  * Let `first` be the current node
  * Let `second` be `first.next`
* Swap their links
* Move forward by two nodes and repeat

If there’s an odd number of nodes, the last node remains unchanged.

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
import java.util.*;

public class Solution{
    public static ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;

        while(prev.next != null && prev.next.next != null) {
            ListNode first = prev.next;
            ListNode second = first.next;
            
            // swap
            first.next = second.next;
            second.next = first;
            prev.next = second;
            
            // move forward
            prev = first;
        }
        
        return dummy.next;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
