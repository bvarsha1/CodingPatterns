## Count Subarrays With Fixed Bounds

#### Statement

Given an integer array, `nums`, and two integers `minK` and `maxK`, return the number of fixed-bound subarrays.
A subarray in `nums` is called a **fixed-bound** subarray if it satisfies the following conditions:
1. The smallest value in the subarray equals `minK`.
2. The largest value in the subarray equals `maxK`.

> **Note:** A **subarray** is a contiguous sequence of elements within an array.
---

#### ✔️ Constraints

* `2 ≤ nums.length ≤ 10³`
* `1 ≤ nums[i], minK, maxK ≤ 10³`

---

## 🎯 Intuition

We track three important positions while scanning:

| Variable     | Meaning                                                      |
| ------------ | ------------------------------------------------------------ |
| `minKIdx`    | Most recent index where `nums[i] == minK`                    |
| `maxKIdx`    | Most recent index where `nums[i] == maxK`                    |
| `invalidIdx` | Most recent index where number is **outside** `[minK, maxK]` |

A valid subarray ending at index `i` must include:

* At least one `minK`
* At least one `maxK`
* No invalid values after those elements

So for each `i`, we count:

```
valid_subarrays_ending_here = min(minKIdx, maxKIdx) - invalidIdx
```

If both required values exist → add to result
If not → contribute `0`

This gives a linear time counting method from left to right.

---

## ✅ Java Solution

```java
public class Solution {

    public long countSubarrays(int[] nums, int minK, int maxK) {
        long count = 0;
        int minKIdx = -1, maxKIdx = -1, invalidIdx = -1;
        
        for (int i = 0; i < nums.length; i++) {
            
            // Invalid range element
            if (nums[i] < minK || nums[i] > maxK) {
                invalidIdx = i;
            }
            
            // Track most recent minK and maxK
            if (nums[i] == minK) minKIdx = i;
            if (nums[i] == maxK) maxKIdx = i;
            
            // Only count when we have seen both
            if (minKIdx != -1 && maxKIdx != -1) {
                count += Math.max(Math.min(minKIdx, maxKIdx) - invalidIdx, 0);
            }
        }
        
        return count;
    }
}
```
- **Time Complexity: O(n)**
- **Space Complexity: O(1)**