## Minimum Size Subarray Sum

#### **Statement**

Given an array of positive integers, nums, and a positive integer, target, find the minimum length of a contiguous subarray whose sum is greater than or equal to the target. If no such subarray is found, return 0.

---

#### ✔️ Constraints

* `1 ≤ target ≤ 10^4`
* `1 ≤ nums.length ≤ 10^3`
* `1 ≤ nums[i] ≤ 10^3`

---

## 🎯 Intuition

We need the **shortest contiguous subarray** whose sum ≥ `target`.
Because all numbers are **positive**, a **sliding window (two pointers)** works perfectly:

* Maintain a window `[i … j]` and its running `sum`.
* Expand the window by moving the right pointer `j` (adding `nums[j]` to `sum`) until `sum >= target`.
* Once `sum >= target`, try to **shrink** the window from the left (`i`) as much as possible while keeping `sum >= target`. Each shrink step updates the candidate minimum length.
* Continue expanding and contracting — each element enters and leaves the window at most once.

This guarantees **O(n)** time with **O(1)** extra space.

---

## ✅ Java Solution

```java
import java.util.*;

class MinimumSubArraySum{
    public static int minSubArrayLen(int target, int[] nums) {
      int minLen = Integer.MAX_VALUE;
      int i = 0, sum = 0;
      for(int j = 0; j < nums.length; j++) {
        sum += nums[j];
        
        while(sum >= target) {
          minLen = Math.min(minLen, j - i + 1);
          sum -= nums[i];
          i++;
        }
      }
      
      return (minLen == Integer.MAX_VALUE) ? 0 : minLen;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n) — each element is added and removed at most once.
* **Space Complexity:** O(1) — only pointers and counters used.
