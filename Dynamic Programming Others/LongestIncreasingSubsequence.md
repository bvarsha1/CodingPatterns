## Longest Increasing Subsequence

#### Statement

Given an integer array `nums`, return the length of the **longest strictly increasing subsequence**.

A subsequence is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements.

---

#### ✔️ Constraints

* 1 ≤ nums.length ≤ 2500
* -10⁴ ≤ nums[i] ≤ 10⁴

---

## 🎯 Intuition

We want the **longest subsequence** such that:

* Elements are in **increasing order**
* Elements are **not required to be contiguous**

At every index, we must decide:

> What is the longest increasing subsequence **ending at this index**?

This naturally leads to **dynamic programming**, where each position depends on previous positions.

---

## ✅ Java Code (DP Solution)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];

        // Base case
        Arrays.fill(dp, 1);

        // Build dp array
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }

        // Find maximum
        int ans = 0;
        for (int len : dp) {
            ans = Math.max(ans, len);
        }

        return ans;
    }
}
```

---

## ⏱ Complexity

* **Time Complexity:** `O(n²)`
* **Space Complexity:** `O(n)`

---

## 🧩 Notes

* This is a **0/1 Knapsack–style DP**
* Each element can be:
  * Taken (if it increases)
  * Skipped
* The constraint is **order-based**, not weight-based

Often described as:
> **DP on sequences with ordering constraint**