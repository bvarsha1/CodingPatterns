## Reverse Nodes in Even Length Groups

#### Statement

Given the head of a linked list, the nodes in it are assigned to each group in a sequential manner. The length of these groups follows the sequence of natural numbers. Natural numbers are positive whole numbers denoted by `(1, 2, 3, 4...)`.

In other words:

* The `1st` node is assigned to the first group.
* The `2nd` and `3rd` nodes are assigned to the second group.
* The `4th`, `5th`, and `6th` nodes are assigned to the third group, and so on.

Your task is to reverse the nodes in each group with an even number of nodes and return the head of the modified linked list.

Note: The length of the last group may be less than or equal to `1 +` the length of the second to the last group.

---

#### ✔️ Constraints

* `1 ≤ Number of nodes ≤ 500`
* `0 ≤ Node.val ≤ 10^3`

---

## 🎯 Intuition

The list is divided into **groups of increasing size**: `1, 2, 3, 4, ...`

For each group:

* First, determine how many nodes actually exist in the group
* If the group size is **even**, reverse that group
* If the group size is **odd**, leave it unchanged
* Carefully reconnect the reversed (or untouched) group with the rest of the list

Key points:

* Group sizes may be smaller near the end of the list
* Only pointer manipulation is allowed
* Extra space must remain constant

---

## ✅ Java Solution

```java
// Definition for a Linked List node
// class LinkedListNode {
//     public int data;
//     public LinkedListNode next;
//     public LinkedListNode(int data) {
//         this.data = data;
//         this.next = null;
//     }
// }

public class ReverseNodes {
    public static LinkedListNode reverseEvenLengthGroups(LinkedListNode head) {
        // having 1 or 2 nodes
        if (head.next == null || head.next.next == null) return head;

        LinkedListNode node = head;
        int group = 1;

        // while the current node and its next are not null
        while (node != null && node.next != null) {
            group++;

            // count nodes in the current group
            // node is the last node of the previous group
            LinkedListNode temp = node.next;
            int nodesCount = 0;
            while (nodesCount < group && temp != null) {
                temp = temp.next;
                nodesCount++;
            }

            // check if group length is even
            if (nodesCount % 2 == 0) {
                // reverse the group
                LinkedListNode curr = node.next;
                LinkedListNode prev = null;

                for (int i = 0; i < nodesCount; i++) {
                    LinkedListNode next = curr.next;
                    curr.next = prev;
                    prev = curr;
                    curr = next;
                }

                // reconnect reversed group
                LinkedListNode tail = node.next;
                tail.next = curr;
                node.next = prev;
                node = tail;
            } else {
                // skip nodes in odd-length group
                for (int i = 0; i < nodesCount; i++) {
                    node = node.next;
                }
            }
        }
        return head;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)