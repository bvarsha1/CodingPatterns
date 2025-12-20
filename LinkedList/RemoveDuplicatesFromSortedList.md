## Remove Duplicates from Sorted List

#### Statement

Given the head of a sorted linked list, remove all duplicates such that each element appears only once, and return the list in sorted order.

---

#### ✔️ Constraints

* `0 ≤ n ≤ 300`, where `n` is the number of nodes in the list.
* `-100 ≤ Node.val ≤ 100`
* The list is guaranteed to be sorted in ascending order.

---

## 🎯 Intuition

Since the linked list is already **sorted**, any duplicate values will always appear **next to each other**.

We can:

* Traverse the list using a single pointer
* Compare the current node with the next node
* If both values are the same, skip the next node
* Otherwise, move forward

This way, each value appears exactly once, and the list remains sorted.

---

## ✅ Java Solution

```java
import java.util.*;
import ds_v1.LinkedList.ListNode;

// class ListNode {
//     int val;
//     ListNode next;

//     // Constructor
//     public ListNode(int val) {
//         this.val = val;
//         this.next = null;
//     }
// }


public class Solution 
{
    public static ListNode removeDuplicates(ListNode head) 
    {   
        ListNode curr = head;
        while(curr != null && curr.next != null) {
            if(curr.val == curr.next.val) {
                // skip duplicate
                curr.next = curr.next.next;
            } else {
                curr = curr.next;
            }
        }
        return head;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
