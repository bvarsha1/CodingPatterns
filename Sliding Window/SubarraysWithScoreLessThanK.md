## Count Subarrays With Score Less Than K

#### **Statement**

An array's **score** is defined as the sum of the array elements multiplied by its length.

For example, if the array is `[2, 1, 5]`, its score is:

```
(2 + 1 + 5) × 3 = 24
```

You are given an array of positive integers, `nums`, and a positive integer `k`.
Your task is to **count and return the number of non-empty subarrays** of `nums` whose **score is strictly less than `k`**.

> A subarray is a contiguous sequence of elements within an array.

---

#### ✔️ Constraints

* `1 ≤ nums.length ≤ 10^3`
* `1 ≤ nums[i] ≤ 10^3`
* `1 ≤ k ≤ 10^5`

---

## 🎯 Intuition

We need to count subarrays whose:

```
(sum of subarray) × (length of subarray) < k
```

This can be efficiently computed using a **sliding window**:

* Expand the right pointer `r`, adding `nums[r]` to the running sum.
* For every window `[l..r]`, check whether:

```
windowSum × (r - l + 1) < k
```

* If the score becomes **≥ k**, we shrink the window from the left.
* Every time the window is valid, all subarrays ending at `r` and starting anywhere between `l..r` are valid, contributing:

```
(r - l + 1)
```

This yields an O(n) sliding-window solution since each index enters/exits the window once.

---

## ✅ Java Solution

```java
public class Solution {
    public long countSubarrays(int[] nums, long k) {
        long count = 0;
        long windowSum = 0;
        int l = 0;

        for (int r = 0; r < nums.length; r++) {
            windowSum += nums[r];

            // shrink window while score >= k
            while (windowSum * (r - l + 1) >= k) {
                windowSum -= nums[l];
                l++;
            }

            // number of valid subarrays ending at r
            count += (r - l + 1);
        }

        return count;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n) — sliding window, each element processed a constant number of times.
* **Space Complexity:** O(1) — only variables for sum and pointers.
