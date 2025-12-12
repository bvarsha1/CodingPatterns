Got it, Varsha — **thank you for the clear sample.**
## **Remove Covered Intervals**

#### **Statement**

Given an array of intervals, where each interval is represented as intervals[i] = [lᵢ, rᵢ) (indicating the range from lᵢ to rᵢ, inclusive of lᵢ and exclusive of rᵢ), remove all intervals that are completely covered by another interval in the list. Return the count of intervals that remain after removing the covered ones.

> Note: An interval [a, b) is considered covered by another interval [c, d) if and only if **c ≤ a and b ≤ d**.

---

#### ✔️ Constraints
 - `1 <= intervals.length <= 1000`
 - `intervals[i].length == 2`
 - `0 <= lᵢ < rᵢ <= 10^5`
 - All the given intervals are unique.


---

## 🎯 Intuition

To identify intervals that are covered:

* **Sort** the intervals by:

  * start ascending, and
  * if starts are equal → end **descending**
    This ensures that any interval capable of covering others appears first.

* Sweep through the sorted list while tracking the largest `prevEnd` seen so far.

  * If current interval’s end `≤ prevEnd` → it is fully covered.
  * Else → it is not covered; count it and update `prevEnd`.

This greedy sweep efficiently detects covered intervals.

---

## ✅ Java Solution

```java
import java.util.Arrays;

public class Solution {
    public int removeCoveredIntervals(int[][] intervals)
    {
        if(intervals.length <= 1) return intervals.length;
        
        Arrays.sort(intervals, (a, b) -> {
           if(a[0] == b[0]) return Integer.compare(b[1], a[1]);
           else return Integer.compare(a[0], b[0]);
        });
        
        int count = 0;
        int prevEnd = 0;
        
        for(int[] i : intervals) {
            if(i[1] > prevEnd) {
                count++;
                prevEnd = i[1];
            }
        }
        
        return count;
   }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  Sorting requires **O(n log n)**; the sweep is **O(n)** → total **O(n log n)**.

* **Space Complexity:**
  **O(1)** extra space aside from input (sorting is in-place).
