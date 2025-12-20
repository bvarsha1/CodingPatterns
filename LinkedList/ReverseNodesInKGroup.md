## Reverse Nodes in k-Group

#### Statement

The task is to reverse the nodes in groups of `k` in a given linked list, where `k` is a positive integer, and at most the length of the linked list. If any remaining nodes are not part of a group of `k`, they should remain in their original order.

It is not allowed to change the values of the nodes in the linked list. Only the order of the nodes can be modified.

Note: Use only `O(1)` extra memory space.

---

#### ✔️ Constraints

Let `n` be the number of nodes in a linked list.

* `1 ≤ k ≤ n ≤ 500`
* `0 ≤ Node.value ≤ 1000`

---

## 🎯 Intuition

We reverse the linked list **in chunks of size `k`** instead of reversing the entire list at once.

The idea:

* Traverse the list and check whether there are at least `k` nodes left
* If yes → reverse those `k` nodes
* Connect the reversed group to the previously processed part
* Move forward and repeat

If fewer than `k` nodes remain at the end:

* Leave them unchanged, as required

This approach ensures:

* Node values are untouched
* Only pointers are rearranged
* Constant extra space is used

---

## ✅ Java Solution

```java
// Definition for a Linked List nod
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

public class Solution
{
    public static ListNode reverseKGroups(ListNode head, int k) {
        if(head == null) return head;
        
        ListNode end = head;
        int count = 0;
        while(end != null && count < k) {
            end = end.next;
            count++;
        }
        
        if(count < k) return head;
        
        ListNode prev = null, next = null, curr = head;
        
        while(curr != end) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        
        head.next = reverseKGroups(curr, k);
        return prev;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
