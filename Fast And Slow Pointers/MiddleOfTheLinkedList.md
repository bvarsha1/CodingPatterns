## Middle of the Linked List

#### Statement

Given the head of a singly linked list, return the **middle node** of the linked list.

If the number of nodes is **even**, there will be *two* middle nodes —
➡️ return the **second** middle node.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 100` — number of nodes
* `1 ≤ Node.value ≤ 100`
* `head ≠ NULL`

---

## 🎯 Intuition

Use the **Fast & Slow Pointer** technique:

| Pointer | Moves             |
| ------- | ----------------- |
| `slow`  | 1 step at a time  |
| `fast`  | 2 steps at a time |

When `fast` reaches the end:

➡️ `slow` will be at the **middle**.
This automatically returns the **second** middle when count is even ✔️

---

## ✅ Java Solution

```java
public class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
