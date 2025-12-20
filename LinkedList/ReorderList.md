## Reorder List

#### Statement

Given the head of a singly linked list, reorder the list as if it were folded on itself. For example, if the list is represented as follows:

`L0 → L1 → L2 → … → Ln−2 → Ln−1 → Ln`

This is how you’ll reorder it:

`L0 → Ln → L1 → Ln−1 → L2 → Ln−2 → …`

You don’t need to modify the values in the list’s nodes; only the links between nodes need to be changed.

---

#### ✔️ Constraints

* The range of number of nodes in the list is `[1, 500]`
* `-5000 ≤ Node.value ≤ 5000`

---

## 🎯 Intuition

The reordering pattern alternates between:

* The **first half** of the list (from the start)
* The **second half** of the list (from the end)

To achieve this efficiently:

1. **Find the middle** of the linked list using slow and fast pointers
2. **Reverse the second half** of the list
3. **Merge the two halves alternately**, one node at a time

This approach:

* Does not change node values
* Only updates pointers
* Uses constant extra space

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
    public static ListNode reorderList(ListNode head) {
        ListNode slow = head, fast = head;
        
        // 1. find middle
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // 2. reverse second half
        ListNode prev = null, curr = slow;
        while(curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        
        // 3. reorder
        ListNode first = head, second = prev;
        while(second.next != null) {
            ListNode temp1 = first.next, temp2 = second.next;
            first.next = second;
            second.next = temp1;
            first = temp1;
            second = temp2;
        }
        
        return head;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)