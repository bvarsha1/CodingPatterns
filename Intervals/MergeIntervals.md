## **Merge Intervals**

#### **Statement**

You are given an array of closed intervals, `intervals`, where each interval has a start time and an end time, represented as `intervals[i] = [startᵢ, endᵢ]`.
Your task is to **merge all overlapping intervals** and return a new array consisting only of **non-overlapping** merged intervals.

---

#### ✔️ Constraints

* `1 ≤ intervals.length ≤ 10^3`
* `intervals[i].length == 2`
* `0 ≤ startᵢ ≤ endᵢ ≤ 10^4`

---

## 🎯 Intuition

To merge intervals:

* First **sort** intervals by their start time so that potential overlaps appear next to each other.
* Use a list (e.g., `LinkedList`) to store merged intervals:

  * If the current interval does **not** overlap with the last interval in the result → simply add it.
  * If it **does** overlap → merge by updating the end time to the max of both ends.
* This ensures a single left-to-right scan merges all overlapping ranges cleanly.

Sorting guarantees intervals are processed in order, and merging is done in linear time after sorting.

---

## ✅ Java Solution

```java
import java.util.*;

class Solution {
    public static int[][] mergeIntervals(int[][] intervals) {
        Arrays.sort(intervals, (int[] a, int[] b) -> Integer.compare(a[0], b[0]));
        if(intervals.length <= 1) return intervals;
        LinkedList<int[]> ans = new LinkedList<>();

        for(int[] i : intervals) {
            if(ans.isEmpty() || ans.getLast()[1] < i[0]) {
                // non-overlapping
                ans.add(i);
            } else {
                // overlapping
                ans.getLast()[1] = Math.max(ans.getLast()[1], i[1]);
            }
        }
        
        // can also write -
        // return merged.toArray(new int[merged.size()][]);
        return ans.toArray(new int[][]{});
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  Sorting takes **O(n log n)**, and merging takes **O(n)** → overall **O(n log n)**.

* **Space Complexity:**
  **O(n)** for storing merged intervals in the output list.
