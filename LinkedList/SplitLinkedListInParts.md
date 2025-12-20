## Split Linked List in Parts

#### Statement

You are given head of a singly linked list and an integer, `k`. Your task is to split the linked list into `k` consecutive parts.

- Each part should have a size as equal as possible, with the difference between any two parts being at most `1`.
- If the list cannot be evenly divided, the earlier parts should have more nodes than the later ones.
- Any parts that cannot be filled with nodes should be represented as `NULL`.
- The parts must appear in the same order as in the input-linked list.

Return an array of the `k` parts, maintaining the specified conditions.

---

#### ✔️ Constraints

* The number of nodes in the list is in the range `[0, 10³]`
* `0 ≤ Node.val ≤ 10³`
* `1 ≤ k ≤ 50`

---

## 🎯 Intuition

First, determine the **total length** of the linked list.

Then:

* Each part will have a base size of `n / k`
* The first `n % k` parts will get **one extra node**
* Traverse the list and cut it into parts accordingly

If there are fewer nodes than `k`, the remaining parts will simply be `NULL`.

This ensures:

* Parts are as equal in size as possible
* Earlier parts are larger when division isn’t even
* Order of nodes is preserved

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
    public static ListNode[] splitListToParts(ListNode head, int k) {
        ListNode curr = head;
        int count = 0;
        while(curr != null) {
            count++;
            curr = curr.next;
        }
        
        int split = count / k;
        int remainder = count % k;
        
        ListNode[] ans = new ListNode[k];
        curr = head;
        for(int i = 0; i < k; i++) {
            ans[i] = curr;
            int currSize = split + ((remainder > 0) ? 1 : 0);
            remainder--;
            
            for(int j = 0; j < currSize - 1; j++) {
                curr = curr.next;
            }
            
            if(curr != null) {
                ListNode temp = curr.next;
                curr.next = null;
                curr = temp;
            }
        }
        
        return ans;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1) (excluding the output array)
