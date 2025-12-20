## Swapping Nodes in a Linked List

#### Statement

Given the head of a linked list and an integer `k`, return the head of the linked list after swapping the values of the `k`th node from the beginning and the `k`th node from the end of the linked list.

Note: We’ll number the nodes of the linked list starting from `1` to `n`.

---

#### ✔️ Constraints

The linked list will have `n` number of nodes.

* `1 ≤ k ≤ n ≤ 500`
* `-5000 ≤ Node.value ≤ 5000`

---

## 🎯 Intuition

To swap the `k`th node from the beginning and the `k`th node from the end:

* Traverse the list to reach the `k`th node from the start
* Then, continue traversing to the end while maintaining another pointer from the head
* When the first pointer reaches the last node, the second pointer will be at the `k`th node from the end

Finally:

* Swap the **values** of the two identified nodes
* No changes to node links are required

This approach avoids extra space and works in a single pass after locating the first `k`th node.

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
    public static ListNode swapNodes(ListNode head, int k) {
        // Replace this placeholder return statement with your code
        if(head == null) return head;
        
        int count = 1;
        ListNode node = head;
        while(count < k) {
            node = node.next;
            count++;
        }
        
        ListNode front = node, back = head;
        while(node.next != null) {
            node = node.next;
            back = back.next;
        }
        
        int temp = front.val;
        front.val = back.val;
        back.val = temp;
        
        return head;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
