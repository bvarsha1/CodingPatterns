## **Interval List Intersections**

#### **Statement**

You are given two lists of closed intervals, `intervalListA` and `intervalListB`.
Your task is to return the **intersection** of the two interval lists.

Each interval is represented as `[start, end]`:

* `intervalListA[i] = [startᵢ, endᵢ]`
* `intervalListB[j] = [startⱼ, endⱼ]`

The intersection of two closed intervals *i* and *j* is:

* an **empty set** if they do not overlap, or
* a **closed interval** `[max(startᵢ, startⱼ), min(endᵢ, endⱼ)]` if they do overlap.

Both interval lists are **pairwise disjoint** and **sorted** in ascending order.

---

#### ✔️ Constraints

* `0 ≤ intervalListA.length, intervalListB.length ≤ 1000`
* `intervalListA.length + intervalListB.length ≥ 1`
* `0 ≤ startᵢ < endᵢ ≤ 10^9`
* `endᵢ < startᵢ₊₁` (A is disjoint & sorted)
* `0 ≤ startⱼ < endⱼ ≤ 10^9`
* `endⱼ < startⱼ₊₁` (B is disjoint & sorted)

---

## 🎯 Intuition

Because both interval lists are sorted and non-overlapping within themselves, we can use a **two-pointer technique**:

1. Let pointers `i` and `j` track positions in lists A and B.
2. For the current intervals:

   * Compute the **maximum start**
     `maxStart = max(A[i].start, B[j].start)`
   * Compute the **minimum end**
     `minEnd = min(A[i].end, B[j].end)`
3. If `maxStart ≤ minEnd`, the intervals overlap → add intersection.
4. Move forward in the list whose interval ends *earlier*, because the later-ending interval may still intersect with future intervals.

This ensures all intersections are captured in a single linear scan.

---

## ✅ Java Solution

```java
import java.util.*;

class Solution {

    public static int[][] intervalsIntersection(int[][] intervalsA, int[][] intervalsB) {
        List<int[]> intersection = new LinkedList<int[]>();
        
        int i = 0, j = 0;
        int m = intervalsA.length, n = intervalsB.length;

        // Compute intersections until one list is exhausted
        while (i < m && j < n) {
            int maxStart = Math.max(intervalsA[i][0], intervalsB[j][0]);
            int minEnd   = Math.min(intervalsA[i][1], intervalsB[j][1]);

            // If maxStart <= minEnd, intervals overlap
            if (maxStart <= minEnd) {
                intersection.add(new int[] { maxStart, minEnd });
            }

            // Move the pointer whose interval ends first
            if (intervalsA[i][1] < intervalsB[j][1]) {
                i++;
            } else {
                j++;
            }
        }

        return intersection.toArray(new int[][]{});
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  `O(m + n)` — each interval from both lists is visited at most once.

* **Space Complexity:**
  `O(k)` — where `k` is the number of intersection intervals stored in the output.
