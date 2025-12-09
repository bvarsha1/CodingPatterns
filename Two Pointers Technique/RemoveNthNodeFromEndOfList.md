## Remove Nth Node From End of Linked List

Given the head of a singly linked list and an integer `n`, remove the **nth node from the end** of the list and return the head of the **modified list**.

---

### Constraints
- Number of nodes: `1 ≤ k ≤ 10³`
- Node values: `−10³ ≤ Node.value ≤ 10³`
- Remove index: `1 ≤ n ≤ k`

---

## Intuition

To delete the nth node from the end, we need to know its position from the start.

Instead of calculating the total length and doing two traversals, we can:
- Use **two pointers** with a **fixed gap of n**
- Move the right pointer `n` steps first
- Then move both pointers together until right pointer reaches the end
- The left pointer will then point **right before** the node to delete

This gives us an **efficient single-pass** solution.

---

## Path to Intuition

1. Create two pointers `l` and `r`, starting at head
2. Move `r` forward `n` steps
3. If `r` becomes null → it means we must delete the **head** node
4. Move both pointers until `r.next == null`
5. Skip the target node: `l.next = l.next.next`

---

## Java Solution

```java
import java.util.*;

class RemoveNthNode {
    public static LinkedListNode removeNthLastNode(LinkedListNode head, int n) {
        LinkedListNode l = head, r = head;
        
        // move r by n steps
        for(int i = 0; i < n; i++) {
          if(r != null) {
            r = r.next;
          }
        }
        
        if (r == null) {
          return head.next; // if we remove head, ll starts from head.next
        }
        
        while(r.next != null) {
          l = l.next;
          r = r.next;
        }
        
        l.next = l.next.next;
        
        return head;
    }
}
```
- Time Complexity: O(n)
- Space Complexity: O(1)