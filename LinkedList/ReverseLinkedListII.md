## Reverse Linked List II

#### Statement

Given a singly linked list with `n` nodes and two positions, `left` and `right`, the objective is to reverse the nodes of the list from left to right. Return the modified list.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 500`
* `-5000 ≤ node.val ≤ 5000`
* `1 ≤ left ≤ right ≤ n`

---

## 🎯 Intuition

Instead of reversing the entire linked list, we only need to **reverse a specific portion** from position `left` to `right`.

Key idea:

* Use a **dummy node** to simplify edge cases (like reversing from the head)
* Traverse the list until just before position `left`
* Reverse exactly `(right - left + 1)` nodes
* Reconnect the reversed portion back to the untouched parts of the list

This approach:

* Modifies only pointers (not values)
* Uses constant extra space
* Runs in a single pass over the list

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

public class Solution
{
    public static ListNode reverseBetween(ListNode head, int left, int right)
    {
        if(head == null || head.next == null || left == right) return head;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode prevLeft = dummy;
        for(int i = 0; i < left - 1; i++) {
            prevLeft = prevLeft.next;
        }
        
        ListNode curr = prevLeft.next;
        ListNode prev = null;
        
        for(int i = 0; curr != null && i < right - left + 1; i++) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        
        prevLeft.next.next = curr;
        prevLeft.next = prev;
        
        return dummy.next;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
