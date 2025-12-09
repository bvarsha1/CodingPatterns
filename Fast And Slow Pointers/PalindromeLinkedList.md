## **Palindrome Linked List**

#### **Statement**

Given the head of a linked list, check whether the linked list is a palindrome or not.
Return **TRUE** if the linked list is a palindrome; otherwise, return **FALSE**.

> **Note:** The original structure of the linked list must remain unchanged before and after the checking process.

---

#### **Constraints**

Let **n** be the number of nodes in the linked list:

* **1 ≤ n ≤ 500**
* **0 ≤ Node.value ≤ 9**

---

## 🎯 Intuition

To check if a linked list is a palindrome, we need to compare values from the beginning and the end at the same time. But since a singly linked list can only move forward, we cannot directly access the last nodes.

So, we use a smart trick to make the list easier to compare:

1. **Find the Middle of the List**

   * Use the **fast and slow pointer** method:

     * `fast` moves 2 steps at a time
     * `slow` moves 1 step at a time
   * When `fast` reaches the end, `slow` will be at the **middle** of the list.

2. **Reverse the Second Half**

   * Starting from the `slow` pointer, we **reverse** the second half of the list in-place.
   * This gives us two lists that both move **forward**:

     * First half: normal direction from `head`
     * Second half: reversed direction from `slow`

3. **Compare Both Halves**

   * Use two pointers:

     * One from `head`
     * One from the reversed second half
   * Compare values one by one:

     * If all match → It’s a **palindrome**
     * If any mismatch → **Not** a palindrome

This allows us to check the palindrome pattern efficiently without extra memory.

---

## Java Solution

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

public class Solution{
    public static boolean palindrome(ListNode head) {
        if(head == null || head.next == null) return true;
        ListNode slow = head, fast = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // reverse the seq from slow ptr
        ListNode rHead = reverse(slow);
        
        // compare
        return compare(head, rHead);
    }
    
    public static ListNode reverse(ListNode head) {
        ListNode prev = null, curr = head, temp = null;
        
        while(curr != null) {
            temp = curr.next;
            curr.next = prev;
            prev = curr; // prev to become the curr
            curr = temp; // curr to move forward
        }
        
        return prev;
    }
    
    public static boolean compare(ListNode left, ListNode right) {
        while(left != null && right != null) {
            if(left.val != right.val) return false;
            left = left.next;
            right = right.next;
        }
        
        return true;
    }
}
```
#### ⏱️ Complexity

- **Time Complexity:** **O(n)**  
    We:
    1. Traverse the list to find the middle
    2. Reverse the second half
    3. Compare both halves  
        Each step takes linear time.
        
- **Space Complexity:** **O(1)**  
    We only manipulate existing pointers and do not use any extra data structures.