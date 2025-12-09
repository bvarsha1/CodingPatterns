## Find the Duplicate Number

#### Statement

Given an array of positive numbers, nums, such that the values lie in the range `[1, n]`, inclusive, and that there are `n + 1` numbers in the array, find and return the duplicate number present in nums. There is only one repeated number in nums, but it may appear more than once in the array.

Note: You cannot modify the given array nums. You have to solve the problem using only constant extra space.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 10^3`
* `nums.length = n + 1`
* `1 ≤ nums[i] ≤ n`
* All the integers in nums are unique, except for one integer that will appear more than once.

---

## 🎯 Intuition

Think of the array like a linked list:

* Each index is a node
* Each value points to the next node → `next = nums[i]`

Since one value repeats → **a cycle must exist**
We can detect this cycle using **Floyd’s Tortoise & Hare algorithm**:

1. Slow → moves 1 step
2. Fast → moves 2 steps
3. When they meet → a cycle is detected
4. Reset one pointer to start, move both 1 step → meeting point = duplicate

This satisfies the constraints:

✔️ No modification of array
✔️ Constant memory
✔️ Linear time

---

## ✅ Java Solution (Floyd’s Cycle Detection)

```java
import java.util.*;

public class FindDuplicate{
   public static int findDuplicate(int[] nums) {
      int slow = 0, fast = 0;
      while(true) {
         slow = nums[slow];
         fast = nums[nums[fast]];
         if(slow == fast) break;
      }
      
      int slow2 = 0;
      while(slow2 != slow) {
         slow = nums[slow];
         slow2 = nums[slow2];
      }
      
      return slow;
   }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)