## Circular Array Loop

#### **Statement**

There is a circular list of non-zero integers called `nums`. Each number in the list tells you how many steps to move forward or backward from your current position:

- If `nums[i]` is positive, move `nums[i]` steps forward.
    
- If `nums[i]` is negative, move `nums[i]` steps backward.
    

As the list is circular:

- Moving forward from the last element takes you back to the first element.
    
- Moving backward from the first element takes you to the last element.
    

A **cycle** in this list means:

1. You keep moving according to the numbers, and you end up repeating a sequence of indexes.
    
2. All numbers in the cycle have the same sign (either all positive or all negative).
    
3. The cycle length is greater than 1 (it involves at least two indexes).
    

Return `true` if such a cycle exists in the list or `false` otherwise.

---

#### ✔️ Constraints

- 1≤1≤ `nums.length` ≤103≤103
- −5000≤−5000≤ `nums[i]` ≤5000≤5000
- `nums[i]` !=0!=0

---

## 🎯 Intuition

We will use **Floyd’s Fast & Slow Pointer** approach on index movement:

* Move **slow** by one step and **fast** by two steps
* Ensure movement direction is consistent (all positive or all negative)
* If fast and slow pointers meet → a **cycle exists**
* Reject **self-loops** (cycle length must be > 1)
* Mark visited numbers as **0** to avoid reprocessing

---

## ✅ Java Solution

```java
public class Solution {
    public static boolean circularArrayLoop(int[] nums) {
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) continue;

            boolean forward = nums[i] > 0;
            int slow = i, fast = i;

            while (true) {
                slow = nextIndex(nums, forward, slow);
                fast = nextIndex(nums, forward, fast);

                if (fast != -1) {
                    fast = nextIndex(nums, forward, fast);
                }

                if (slow == -1 || fast == -1 || slow == fast)
                    break;
            }

            if (slow != -1 && slow == fast)
                return true;

            // mark path as visited
            int idx = i;
            while (nums[idx] != 0) {
                int next = nextIndex(nums, nums[idx] > 0, idx);
                nums[idx] = 0;
                idx = next;
                if (idx == -1) break;
            }
        }

        return false;
    }

    private static int nextIndex(int[] nums, boolean forward, int current) {
        boolean dir = nums[current] > 0;
        if (dir != forward) return -1;

        int n = nums.length;
        int next = ((current + nums[current]) % n + n) % n;

        if (next == current) return -1;

        return next;
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)