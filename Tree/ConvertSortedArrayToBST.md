## Convert Sorted Array to Binary Search Tree

#### Statement

Given an array of integers, `nums`, sorted in ascending order, your task is to construct a height-balanced binary search tree (BST) from this array.

In a height-balanced BST, the difference of heights of the left subtree and right subtree of any node is not more than 1.

> **Note:** There can be multiple valid BSTs for a given input.

---

#### ✔️ Constraints

* `1 ≤ nums.length ≤ 10^3`
* `−10^4 ≤ nums[i] ≤ 10^4`
* `nums` is sorted in strictly ascending order.

---

## 🎯 Intuition

To construct a **height-balanced BST** from a sorted array:

* The **middle element** of the array should become the **root**
* The **left half** of the array forms the **left subtree**
* The **right half** of the array forms the **right subtree**

Why this works:

* Choosing the middle ensures that the number of nodes on the left and right are as equal as possible
* Recursively applying this logic guarantees:

  * BST property (left < root < right)
  * Height balance at every node

This is a classic **divide-and-conquer DFS** approach.

---

## ✅ Java Solution

```java
import java.util.*;
import ds_v1.BinaryTree.TreeNode;

// Definiton of a binary tree node class
// class TreeNode<T> {
//     T data;
//     TreeNode<T> left;
//     TreeNode<T> right;
//
//     TreeNode(T data) {
//         this.data = data;
//         this.left = null;
//         this.right = null;
//     }
// }

public class Solution {

    public static TreeNode<Integer> sortedArrayToBST(int[] nums) {
        return dfs(nums, 0, nums.length - 1);
    }

    public static TreeNode<Integer> dfs(int[] nums, int s, int e) {
        // base case
        if (s > e) return null;

        // choose middle element
        int m = (s + e) / 2;

        // create root node
        TreeNode<Integer> node = new TreeNode<>(nums[m]);

        // recursively build left and right subtrees
        node.left = dfs(nums, s, m - 1);
        node.right = dfs(nums, m + 1, e);

        return node;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(log n)` (recursive call stack)
