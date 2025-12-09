## **Intersection of Two Linked Lists**

#### **Problem Statement**

You are given the heads of two singly linked lists, **headA** and **headB**, and must determine whether the two lists intersect.

If they intersect, return the **node where the intersection begins**.
If not, return **`null`**.

> **Note:** Two linked lists intersect **only if they share the exact same node in memory**, not just a node with the same value.

---

#### **Constraints**

- `1 ≤ node.val ≤ 10^5`
- The number of nodes of listA is in the m.
- The number of nodes of listB is in the n.
- `1 ≤ m, n ≤ 10^3`
---

## **Intuition — Why the Two Pointer Trick Works**

Imagine:

* List A length = `m`
* List B length = `n`
* Intersection starts after `x` nodes from the start of list A and `y` nodes from B
  such that:

```
m - x == n - y   (common tail length)
```

The technique:

* Pointer **a** traverses: `A → B`
* Pointer **b** traverses: `B → A`

This makes both pointers travel exactly the same total distance:

```
Distance traveled by a = m + n
Distance traveled by b = n + m
```

So:

✔ They will **either meet at the intersection node**
❌ Or both will reach **null** at the same time (no intersection)

No extra memory used, just pointer switching — brilliant and elegant.

---

## **Java Solution**

```java
/*
Input Format (As handled by the judge. Your program is not given these inputs)
listA – The first linked list.
listB – The second linked list.
skipA – The number of nodes to skip in listA (starting from the head) to reach the intersection node.
skipB – The number of nodes to skip in listB (starting from the head) to reach the intersection node.
intersectVal – The value of the node where the intersection begins. Set to 0 if the two linked lists do not intersect.

Using these inputs, the judge constructs the linked lists and connects them at the intersection point, if one exists.
The two list heads, headA and headB, are then passed to your function.
If your function correctly identifies and returns the intersecting node, the solution is accepted.
*/

import ds_v1.LinkedList.ListNode;

public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA;
        ListNode b = headB;

        while (a != b) {
            a = (a == null) ? headB : a.next;
            b = (b == null) ? headA : b.next;
        }

        return a;  // Either intersection node or null
    }
}
```
#### ⏱️ Complexity
- **Time Complexity:** O(m + n)
- **Space Complexity:** O(1)
