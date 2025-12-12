## **Employee Free Time**

#### **Statement**

You’re given a list containing the schedules of multiple employees. Each person’s schedule is a list of non-overlapping intervals in sorted order. An interval is specified with the start and end time, both being positive integers. 

Your task is to find the list of finite intervals representing the free time for all the employees.

> Note: The common free intervals are calculated between the earliest start time and the latest end time of all meetings across all employees.

---

#### ✔️ Constraints

* `1 ≤ schedule.length , schedule[i].length ≤ 50`
* `0 ≤ interval.start < interval.end ≤ 10^8`, where interval is any interval in the list of schedules.

---

## 🎯 Intuition

We need to find free time across multiple employees, given that each employee's schedule contains sorted and non-overlapping intervals. Since all employees have their interval lists sorted, a **min-heap** (priority queue) is ideal for always retrieving the next earliest start time across all employees.

Steps:

* Push the first interval of each employee into a min-heap (ordered by start time).
* Track a variable `prev` that marks the end of the last processed meeting.
* Pop the earliest-start interval from the heap:

  * If its start is greater than `prev`, then the gap `[prev, interval.start]` is a free interval.
  * Update `prev = max(prev, interval.end)` to maintain the merged busy timeline.
* Push the next interval for that employee into the heap.
* Repeat until the heap becomes empty.

The gaps detected while merging represent all common free time intervals.

---

## ✅ Java Solution

```java
import java.util.*;

class Solution {
    public static List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        List<Interval> ans = new ArrayList<Interval>();
        
        for (int i = 0; i < schedule.size(); i++) {
            List<Interval> employeeSchedule = schedule.get(i);
            // start times of employeeSchedule, employee index, Interval index
            heap.offer(new int[] { employeeSchedule.get(0).start, i, 0 });
        }
        
        int prev = heap.peek()[0];
        
        while (!heap.isEmpty()) {
            int[] curr = heap.poll();
            int i = curr[1]; // employee index
            int j = curr[2]; // interval index
            Interval interval = schedule.get(i).get(j);
            
            if (interval.start > prev) {
                ans.add(new Interval(prev, interval.start));
            }
            prev = Math.max(prev, interval.end);
            
            if (j + 1 < schedule.get(i).size()) {
                heap.offer(new int[] { schedule.get(i).get(j + 1).start, i, j + 1 });
            }
        }
        
        return ans;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  `O(N log k)` where `N` is the total number of intervals across all employees and `k` is the number of employees.

* **Space Complexity:**
  `O(k)` for the heap plus `O(F)` for storing free intervals.
