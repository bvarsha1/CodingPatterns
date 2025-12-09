## Maximum Score of a Good Path in Two Sorted Arrays

#### Statement

You are given two sorted arrays of distinct integers, `nums1` and `nums2`.

A valid path is constructed according to the following rules:

1. You start at the index `0` of either `nums1` or `nums2`.
2. From there, you must traverse the chosen array **left to right**.
3. If you encounter a number that exists in **both** arrays (a common element),
   you may **switch** to the other array at that point and continue forward.

> A common element can only be counted **once** in the total path score, regardless of which array it appears in.

The **score** of a path is the sum of all **unique elements visited** during traversal.

Your task: return the **maximum possible score modulo** `10⁹ + 7`.

---

#### ✔️ Constraints

* `1 ≤ nums1.length, nums2.length ≤ 10⁵`
* `1 ≤ nums1[i], nums2[i] ≤ 10⁷`
* All elements in nums1 and nums2 are strictly increasing.


---

## 🎯 Intuition

We simulate walking through both arrays **at the same time** using two pointers:

| Variable | Meaning                              |
| -------- | ------------------------------------ |
| `sum1`   | Best score if we continue in `nums1` |
| `sum2`   | Best score if we continue in `nums2` |
| `i, j`   | Pointers for `nums1` and `nums2`     |

As long as elements differ:

* If `nums1[i] < nums2[j]` → include nums1 value into `sum1`
* If `nums1[i] > nums2[j]` → include nums2 value into `sum2`

When we hit a **common element**:

We must choose whether to come from `nums1` or `nums2`.
So we take the **max** path so far and continue from there:

```
best = max(sum1, sum2) + currentCommonValue
sum1 = sum2 = best
```

This guarantees the optimal score while respecting the switching rule.

After one pointer finishes, we just add the remaining values from the other array.

---

## ✅ Java Solution

```java
import java.util.Arrays;

public class Solution {
    public static int maxSum(int[] nums1, int[] nums2) {
        long sum1 = 0, sum2 = 0;
        int i = 0, j = 0;
        int m = nums1.length, n = nums2.length;
        long MOD = 1_000_000_007L;

        while (i < m && j < n) {
            if (nums1[i] < nums2[j]) {
                sum1 += nums1[i++];
            } else if (nums1[i] > nums2[j]) {
                sum2 += nums2[j++];
            } else {
                long best = Math.max(sum1, sum2) + nums1[i];
                sum1 = sum2 = best;
                i++;
                j++;
            }
        }

        while (i < m) sum1 += nums1[i++];
        while (j < n) sum2 += nums2[j++];

        return (int)(Math.max(sum1, sum2) % MOD);
    }
}
```
- **Time Complexity:** O(m + n)
- **Space Complexity:** O(1) — only pointers + sums
