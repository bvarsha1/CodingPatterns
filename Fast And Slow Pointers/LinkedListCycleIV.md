## Linked List Cycle IV

#### **Statement**

Given the head of a singly linked list, **detect and remove any cycle** present in the list.

A **cycle** occurs when a node’s `next` pointer links back to a previous node, forming a loop.
Modify the list **in place** to make it **acyclic**, while preserving the original order of nodes.

If there is **no cycle**, return the list **as is**.

---

#### ✔️ Constraints

* Number of nodes in the list:
  `0 ≤ n ≤ 10^4`
* Node values range:
  `−10^5 ≤ Node.value ≤ 10^5`

---

## 🎯 Intuition

Use **Floyd's Fast & Slow Pointers** to:

1️⃣ Detect a cycle
2️⃣ If pointers meet → a cycle exists
3️⃣ To remove the cycle:

* Move one pointer to `head`
* Move both one step at a time → meeting point = **cycle start**
* Traverse from cycle start until node whose `next` is cycle start
* Set that node’s `next = null` → cycle removed

This ensures the **list remains intact without reordering**.

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
    public static ListNode removeCycle(ListNode head) {
        if (head == null) return head;

        ListNode slow = head, fast = head;

        // Step 1: Detect Cycle
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) {
                // Step 2: Find cycle start node
                slow = head;
                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }

                // 'slow' is now at cycle start
                // Step 3: Break the cycle
                while (fast.next != slow) {
                    fast = fast.next;
                }
                fast.next = null;
            }
        }
        return head;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
