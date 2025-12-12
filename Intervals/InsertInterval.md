## **Insert Interval**

#### **Statement**

You are given a list of non-overlapping intervals, `intervals`, where each interval is represented as `[startᵢ, endᵢ]`, and the list is sorted in ascending order by start time (`startᵢ`).
You are also given another interval, `newInterval = [start, end]`.

Your task is to **insert** `newInterval` into the list such that:

* the list remains sorted by starting times
* the list continues to have **no overlapping intervals**
* any resulting overlapping intervals must be **merged**

Return the updated list of intervals.

**Note:** You do not need to modify the original `intervals` list in place.

---

#### ✔️ Constraints

* `0 ≤ intervals.length ≤ 10^4`
* `intervals[i].length == 2`
* `newInterval.length == 2`
* `0 ≤ startᵢ < endᵢ ≤ 10^4`
* The list of intervals is **sorted ascending** by start time.

---

## 🎯 Intuition

To insert the new interval correctly:

1. **Add all intervals that end before the new interval starts** — they cannot overlap.
2. **Merge all intervals that overlap** with the new interval by:

   * updating the start to `min(current.start, newInterval.start)`
   * updating the end to `max(current.end, newInterval.end)`
3. **Add all intervals that start after the new interval ends** — they also cannot overlap.

Because intervals are already sorted, we only need a **single left-to-right pass**, making this efficient.

---

## ✅ Java Solution

```java
import java.util.*;

class Solution {
  public static int[][] insertInterval(int[][] existingIntervals, int[] newInterval) {
    int n = existingIntervals.length;
    int i = 0;
    LinkedList<int[]> merged = new LinkedList<>();
    // while intervals are non-overlapping with new interval
    while(i < n && existingIntervals[i][0] < newInterval[0]) {
      insertOrMergeInterval(merged, existingIntervals[i]);
      i++;
    }
    
    insertOrMergeInterval(merged, newInterval);
    
    while(i < n) {
      insertOrMergeInterval(merged, existingIntervals[i]);
      i++;
    }
    
    return merged.toArray(new int[][] {});
  }
  
  public static void insertOrMergeInterval(LinkedList<int[]> merged, int[] newInterval) {
    if(merged.isEmpty() || merged.getLast()[1] < newInterval[0]) {
      merged.add(newInterval);
    } else {
      merged.getLast()[1] = Math.max(merged.getLast()[1], newInterval[1]);
    }
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  `O(n)` — each interval is processed exactly once.

* **Space Complexity:**
  `O(n)` — for storing the output list of intervals.
