## Linked List Cycle III

#### **Statement**

Given the head of a linked list, **determine the length of the cycle** present in the list.
If there is **no cycle**, return **0**.

A cycle exists if some node in the linked list can be reached again by continuously following the `next` pointer.

---

#### ✔️ Constraints

* Number of nodes in the list: 
  `0 ≤ n ≤ 10^4`
* Node values range: 
  `−10^5 ≤ Node.value ≤ 10^5`

---

## 🎯 Intuition

We use **Floyd’s Fast & Slow Pointer Algorithm** to:

1️⃣ Detect if a **cycle exists**
2️⃣ If they meet, calculate **cycle length** by:

* Holding one pointer still
* Moving the other pointer step-by-step
* Counting until they meet again → cycle length

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
    public static int cycleLength(ListNode head) {
        if(head == null) return 0;

        ListNode slow = head, fast = head;

        // Step 1: Detect cycle using fast & slow pointers
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast) {
                // Step 2: Cycle found — measure length
                int length = 1;
                fast = fast.next;
                while(fast != slow) {
                    fast = fast.next;
                    length++;
                }
                return length;
            }
        }
        return 0; // No cycle
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)