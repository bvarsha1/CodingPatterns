## Maximum Twin Sum of a Linked List

#### **Statement**

In a linked list with an even number of nodes (`n`), each node at position `i` (using **0-based** indexing) is paired with the node at position `(n − 1 − i)`.
These pairs are called **twins** for all `0 ≤ i < n/2`.

Example when `n = 4`:

| Index | Twin Index |
| ----- | ---------- |
| 0     | 3          |
| 1     | 2          |

The **twin sum** is the sum of a node’s value and its twin’s value.
Given the head of a linked list with an even number of nodes, return the **maximum twin sum** among all pairs.

---

#### ✔️ Constraints

* The list contains an even number of nodes in the range `[2, 10^3]`
* `1 ≤ Node.value ≤ 10^3`

---

## 🎯 Intuition

This problem takes advantage of the **linked list structure** and **twin pairing**:

To efficiently compute twin sums, we should align **first half** and **second half** nodes together.

Steps:

| Step | Action                                          | Why                                                     |
| ---- | ----------------------------------------------- | ------------------------------------------------------- |
| 1️⃣  | Find the middle using fast + slow pointers      | Splits the list in two halves                           |
| 2️⃣  | Reverse the second half                         | Allows pairing corresponding nodes in forward direction |
| 3️⃣  | Compare values in first vs reversed second half | Compute all twin sums                                   |
| 4️⃣  | Track the maximum twin sum                      | Final answer                                            |

This works in:

✔️ Linear time
✔️ Constant space (in-place reversal)
✔️ No extra data structures required

---

## ✅ Java Solution (Two-Pointer + Reverse Approach)

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

class Solution {
    public static int twinSum(ListNode head) {
        ListNode slow = head, fast = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode rHead = reverse(slow);
        
        return maxSum(head, rHead);
    }
    
    public static ListNode reverse(ListNode head) {
        ListNode prev = null, temp = null, curr = head;
        while(curr != null) {
            temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }
        
        return prev;
    }
    
    public static int maxSum(ListNode left, ListNode right) {
        int maxSum = Integer.MIN_VALUE;
        while(left != null && right != null) {
            maxSum = Math.max(maxSum, left.val + right.val);
            left = left.next;
            right = right.next;
        }
        return maxSum;
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
