## Odd Even Linked List

#### Statement

Given the head of a singly linked list, rearrange the nodes so that all nodes at odd indexes appear first, followed by all nodes at even indexes.

The first node in the list is considered at odd index `1`, the second at even index `2`, and so on.

Within the odd group and the even group, the relative order of the nodes must remain the same as in the original list.

Return the head of the reordered linked list.

Note: You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.

---

#### ✔️ Constraints

* The number of nodes in the linked list is in the range `[0, 10³]`
* `-10³ ≤ Node.val ≤ 10³`

---

## 🎯 Intuition

We can rearrange the list by **separating odd-indexed nodes and even-indexed nodes** while traversing the list only once.

Approach:

* Maintain two pointers: one for odd-indexed nodes and one for even-indexed nodes
* Traverse the list, linking odd nodes together and even nodes together
* Finally, connect the end of the odd list to the head of the even list

This preserves:

* The relative order within odd and even groups
* Constant extra space
* Linear time complexity

---

## ✅ Java Solution

```java
import ds_v1.LinkedList.ListNode;
import java.util.*;

public class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null || head.next.next == null) return head;
        
        ListNode odd = head, even = head.next;
        ListNode evenHead = even;
        
        while(even != null && even.next != null) {
            odd.next = odd.next.next; // odd.next = even.next;
            odd = odd.next;
            
            even.next = even.next.next; // even.next = odd.next;
            even = even.next;
        }
        
        odd.next = evenHead;
        
        return head;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
