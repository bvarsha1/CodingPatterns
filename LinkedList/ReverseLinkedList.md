## Reverse Linked List

#### Statement

Given the head of a singly linked list, reverse the linked list and return its updated head.

---

#### ✔️ Constraints

Let `n` be the number of nodes in a linked list.

* `1 ≤ n ≤ 500`
* `-5000 ≤ Node.value ≤ 5000`

---

## 🎯 Intuition

To reverse a singly linked list, we need to **reverse the direction of pointers** between nodes.

At each step:

* Take the current node
* Redirect its `next` pointer to the previous node
* Move forward in the list

By the time we reach the end:

* The last processed node becomes the new head of the reversed list

This can be done efficiently in **one pass** using three pointers.

---

## ✅ Java Solution

#### Iterative approach
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
    public static ListNode reverse(ListNode head) {
        ListNode temp = null, prev = null, curr = head;
        
        while(curr != null) {
            temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }
        
        return prev;
    }
}

```

#### Recursive approach
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
    public static ListNode reverse(ListNode head) {
        if(head == null || head.next == null) return head;
        
        ListNode shead = reverse(head.next);
        head.next.next = head;
        head.next = null;
        
        return shead;
    }
}

```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
