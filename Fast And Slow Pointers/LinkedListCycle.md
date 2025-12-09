## Linked List Cycle

#### Statement

Given the head of a linked list, determine whether the list contains a **cycle**.

A cycle exists if by continuously following the `next` pointers, we eventually revisit a previously visited node.

Return:

* **TRUE** → if a cycle is present
* **FALSE** → if no cycle exists

---

#### ✔️ Constraints

* Let `n` = number of nodes in the linked list
* `0 ≤ n ≤ 500`
* `-10⁵ ≤ Node.value ≤ 10⁵`

---

## 🎯 Intuition

We can use **Floyd’s Cycle Detection** (Fast & Slow pointers):

| Pointer | Moves             |
| ------- | ----------------- |
| `slow`  | 1 step at a time  |
| `fast`  | 2 steps at a time |

📌 If there's **no cycle**, `fast` or `fast.next` will eventually become `null`
📌 If there's a **cycle**, `fast` will eventually **meet** `slow` inside the loop

No extra memory required ➝ **O(1) Space**

---

## ✅ Java Solution

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null)
            return false;
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;            // move 1 step
            fast = fast.next.next;       // move 2 steps
            
            if (slow == fast)
                return true;             // cycle detected
        }
        
        return false;                    // reached null → no cycle
    }
}
```
#### ⏱️ Complexity
* **Time Complexity:** O(n)
* **Space Complexity:** O(1)