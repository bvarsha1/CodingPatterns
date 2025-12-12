## **Meeting Rooms II**

#### **Statement**

We are given an input array of meeting time intervals, `intervals`, where each interval has a start time and an end time.
Your task is to **find the minimum number of meeting rooms** required to hold these meetings.

> Note: The specified end time for each meeting is **exclusive**.

---

#### ✔️ Constraints

* `1 ≤ intervals.length ≤ 10^3`
* `0 ≤ startᵢ < endᵢ ≤ 10^6`

---

## 🎯 Intuition

To determine the minimum number of meeting rooms:

1. **Separate start and end times**:

   * Extract all start times and sort them.
   * Extract all end times and sort them.

2. **Use two pointers** to track ongoing meetings:

   * `i` iterates over start times.
   * `j` iterates over end times.

3. **Sweep through time**:

   * If `start[i] < end[j]`, a new meeting starts **before the earliest meeting ends**, so we need an **additional room**.
   * Otherwise, if `start[i] >= end[j]`, a meeting has ended, so we can **reuse a room** by moving `j` forward.

4. Track the **maximum number of rooms in use** at any point — this is the answer.

> This method works because sorting start and end times allows us to simulate the timeline efficiently.

---

## ✅ Java Solution

```java
import java.util.Arrays;

class Solution {
    public static int minMeetingRooms(int[][] intervals) {
        int n = intervals.length;
        if(n == 0) return 0;

        int[] starts = new int[n];
        int[] ends = new int[n];

        for(int i = 0; i < n; i++) {
            starts[i] = intervals[i][0];
            ends[i] = intervals[i][1];
        }

        Arrays.sort(starts);
        Arrays.sort(ends);

        int rooms = 0, maxRooms = 0;
        int i = 0, j = 0;

        while(i < n) {
            if(starts[i] < ends[j]) {
                rooms++;
                maxRooms = Math.max(maxRooms, rooms);
                i++;
            } else {
                rooms--;
                j++;
            }
        }

        return maxRooms;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n) — sorting both start and end arrays.
* **Space Complexity:** O(n) — for storing start and end times.
