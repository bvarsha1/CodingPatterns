## **Count Days Without Meetings**

#### **Statement**

You are given a positive integer, days, which represents the total number of days an employee is available for work, starting from day 1. You are also given a 2D array, meetings, where each entry `meetings[i] = [startᵢ, endᵢ]` indicates that a meeting is scheduled from day `startᵢ` to day `endᵢ` (both inclusive).

Your task is to count the days when the employee is available for work but has no scheduled meetings.

> **Note:** The meetings may overlap.

---

#### ✔️ Constraints

* `1 ≤ days ≤ 100000`
* `1 ≤ meetings.length ≤ 1000`
* `meetings[i].length == 2`
* `1 ≤ meetings[i][0] ≤ meetings[i][1] ≤ days`

---

## 🎯 Intuition

#### Approach using merge intervals logic, uses O(n) space complexity

We need to compute how many days **do not** fall inside any meeting interval. Because meetings can overlap, we should:

1. **Sort the meeting intervals** by start time.
2. **Merge overlapping intervals** so that all busy days are represented as disjoint continuous blocks.
3. **Count free days** as:

   * gaps between merged intervals
   * plus days before the first interval
   * plus days after the last interval

Example:
If days = 10 and merged intervals are `[2,3]`, `[6,7]`,
then free days = {1,4,5,8,9,10} → 6 days.

This is a classic "merge intervals and count gaps" problem.

#### Approach with O(1) space complexity

We want to count all days _covered by meetings_, then subtract from `days`.

To achieve **O(1) space**, we:

1. **Sort `meetings` in-place** by starting time  
    (Sorting the input array in-place uses constant auxiliary memory)
    
2. **Sweep and merge intervals** without storing them:
    
    - Maintain the current merged interval `[currentStart, currentEnd]`
        
    - If the next meeting overlaps, extend the interval
        
    - If not, add its length to `busyDays` and reset the interval
        
3. Return:
	- `days − busyDays`

---

## ✅ Java Solution

#### Approach using merge intervals logic, uses O(n) space complexity
```java
import java.util.*;

class Solution {
    public static int countDaysWithoutMeetings(int days, int[][] meetings) {
        if (meetings.length == 0) return days;

        // Step 1: Sort intervals
        Arrays.sort(meetings, (a, b) -> Integer.compare(a[0], b[0]));

        // Step 2: Merge intervals
        List<int[]> merged = new ArrayList<>();
        int[] curr = meetings[0];
        
        for (int i = 1; i < meetings.length; i++) {
            int[] next = meetings[i];

            if (next[0] <= curr[1]) {
                // overlapping → merge
                curr[1] = Math.max(curr[1], next[1]);
            } else {
                merged.add(curr);
                curr = next;
            }
        }
        merged.add(curr);

        // Step 3: Count free days
        int freeDays = 0;

        // Before first meeting
        freeDays += merged.get(0)[0] - 1;

        // Between meetings
        for (int i = 1; i < merged.size(); i++) {
            freeDays += merged.get(i)[0] - merged.get(i - 1)[1] - 1;
        }

        // After last meeting
        freeDays += days - merged.get(merged.size() - 1)[1];

        return freeDays;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  `O(n log n)` — sorting the intervals (n = meetings.length).

* **Space Complexity:**
  `O(n)` — storing merged intervals.

---

#### Approach with O(1) space complexity

```java
public int countDays(int days, int[][] meetings) {

    // Step 1: Sort meetings in-place by start time (O(1) space)
    Arrays.sort(meetings, (a, b) -> Integer.compare(a[0], b[0]));

    int busyDays = 0;

    int currentStart = meetings[0][0];
    int currentEnd = meetings[0][1];

    // Step 2: In-place merge sweep
    for (int i = 1; i < meetings.length; i++) {
        int start = meetings[i][0];
        int end = meetings[i][1];

        if (start <= currentEnd) {
            // Overlapping — merge
            currentEnd = Math.max(currentEnd, end);
        } else {
            // No overlap — finalize previous merged interval
            busyDays += (currentEnd - currentStart + 1);

            // Start new merged interval
            currentStart = start;
            currentEnd = end;
        }
    }

    // Add last merged interval
    busyDays += (currentEnd - currentStart + 1);

    // Step 3: Days without meetings
    return days - busyDays;
}
```
#### ⏱️ Complexity

* **Time Complexity:**
  `O(n log n)` — sorting the intervals (n = meetings.length).

* **Space Complexity:**
  `O(1)`