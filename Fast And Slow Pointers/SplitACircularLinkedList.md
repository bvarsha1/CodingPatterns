## Split a Circular Linked List

#### **Statement**

Given a circular linked list `list` of positive integers, split it into **two circular linked lists**.

* The **first circular linked list** should contain the first half of the nodes
  → exactly `⌈list.length / 2⌉` nodes
  → **same order** as original

* The **second circular linked list** contains the remaining nodes
  → **same order** as original

Return an array `answer` of length 2:

| Index       | Meaning                             |
| ----------- | ----------------------------------- |
| `answer[0]` | Head of first circular linked list  |
| `answer[1]` | Head of second circular linked list |

📌 Note: A **circular linked list** is where the **last node points back to the first node**.

---

#### ✔️ Constraints

* `2 ≤ n ≤ 10^3`
* `0 ≤ Node.value ≤ 10^5`
* LastNode.next = FirstNode (circular structure)

---

## 🎯 Intuition

We use **Floyd’s Fast & Slow Pointer** method to locate the **middle** of this circular list:

| Pointer | Moves    | Purpose                |
| ------- | -------- | ---------------------- |
| Slow    | +1 node  | Stops at mid point     |
| Fast    | +2 nodes | Finds end cycle faster |

Once fast reaches or loops around the head again:

1️⃣ `slow` will be at the end of the **first half**
2️⃣ The node after `slow` starts the **second half**
3️⃣ We **break and rewire pointers** to make both lists circular:

* `slow.next → head` (close first list)
* `fast.next → secondHead` (close second list)

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

public class Solution {
    public static ListNode[] splitCircularLinkedList(ListNode head) {
        ListNode slow = head, fast = head;
        while(fast.next != head && fast.next.next != head) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode h1 = head, h2 = slow.next;
        slow.next = h1;
        
        fast = h2;
        while(fast.next != head) {
            fast = fast.next;
        }
        fast.next = h2;
        
        return new ListNode[]{ h1, h2 }; // Return two empty lists as placeholders
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
