## **Data Stream as Disjoint Intervals**

#### **Statement**

You are given a stream of non-negative integers a₁, a₂, …, aₙ. At any point, you need to summarize all numbers seen so far as a list of disjoint intervals.

Your task is to implement the **Summary Ranges** class, where:

* **Constructor:** Initializes the Summary Ranges object with an empty stream.
* **addNum(int value):** Adds the integer value to the stream.
* **getIntervals():** Returns the current summary of numbers as a list of disjoint intervals `[startᵢ, endᵢ]`, sorted by `startᵢ`.

> **Note:** Each number belongs to exactly one interval. Intervals must merge whenever new numbers connect or extend existing ones, and duplicate insertions should not affect the summary.

---

#### ✔️ Constraints

* `0 ≤ value ≤ 10^4`
* At most `3 × 10^4` calls will be made to **addNum()** and **getIntervals()**
* At most `10^2` calls will be made to **getIntervals()**

---

## 🎯 **Intuition**

To maintain disjoint intervals while numbers stream in, we use a **TreeMap** where:

* **Key = interval start**
* **Value = interval end**

This gives us two powerful abilities:

1. **floorEntry(x)** → the interval whose start is ≤ x (possible predecessor)
2. **higherEntry(x)** → the interval whose start is > x (possible successor)

These allow us to check in **O(log n)** time whether `value`:

* is already inside an existing interval,
* should extend an interval on the left,
* should extend an interval on the right,
* or should merge two intervals.


### 🔍 **Step-by-step logic**

When a new number `value` arrives:

#### **1️⃣ Check the previous interval**

We look at `prev = floorEntry(value)`:

* If `prev.end ≥ value`, then the value is **already included**, so **do nothing**.
* If `prev.end == value - 1`, the new number **touches** the previous interval,
  so we extend on the left by setting:

  ```
  newStart = prev.start
  ```

#### **2️⃣ Check the next interval**

We look at `next = higherEntry(value)`:

* If `next.start == value + 1`, the new number **touches** the next interval,
  so we extend on the right by setting:

  ```
  newEnd = next.end
  ```

  and we **remove** the next interval from the map, because it will be merged.

#### **3️⃣ Insert the merged interval**

Finally, we insert:

```
intervals.put(newStart, newEnd)
```

This interval may represent:

* a brand-new interval,
* an extension of the previous interval,
* an extension of the next interval,
* or a merge of both.

### 🧠 **Why this works efficiently**

Because the TreeMap keeps keys sorted, each update takes:

* **O(log n)** time to locate predecessor and successor
* **O(log n)** time to insert or remove intervals

This ensures the summary of all seen numbers is always correctly maintained as **disjoint, sorted, merged intervals**, with optimal performance.

---

## ✅ Java Solution

```java
import java.util.*;
import java.io.*;

class SummaryRanges {
    TreeMap<Integer, Integer> intervals;
    public SummaryRanges() {
        intervals = new TreeMap<>();
    }

    public void addNum(int value) {
        int newStart = value;
        int newEnd = value;
        
        Map.Entry<Integer, Integer> prev = intervals.floorEntry(value);
        Map.Entry<Integer, Integer> next = intervals.higherEntry(value);
        
        if(prev != null) {
            // if prevEnd is greater or equal to value, 
            // we skip adding, as this is already included
            if(prev.getValue() >= value) return;
            
            // if prevEnd is just 1 less than value, set newStart as prevStart
            if(prev.getValue() == value - 1) newStart = prev.getKey();
        }
        
        if(next != null && next.getKey() == value + 1) {
            newEnd = next.getValue();
            intervals.remove(next.getKey());
        }
        
        intervals.put(newStart, newEnd);
    }

    public int[][] getIntervals() {
        int[][] ans = new int[intervals.size()][2];
        int idx = 0;
        for(int start : intervals.keySet()) {
            ans[idx][0] = start;
            ans[idx][1] = intervals.get(start);
            idx++;
        }
        
        return ans;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  Each `addNum()` operation runs in **O(log n)** due to TreeMap lookups and insertions.
  `getIntervals()` runs in **O(k)** where `k` is the number of intervals.

* **Space Complexity:**
  **O(k)** for storing all current disjoint intervals.