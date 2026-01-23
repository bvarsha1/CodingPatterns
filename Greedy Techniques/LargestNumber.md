## Largest Number

#### **Statement**

Given a list of non-negative integers, `nums`, rearrange them so that they form the **largest possible number**.
As the result may be very large, return it as a **string**.

---

#### ✔️ Constraints

* 1 ≤ `nums.length` ≤ 100
* 0 ≤ `nums[i]` ≤ 10^3

---

## 🎯 Intuition

To form the **largest number**, placing numbers like `9`, `91`, `910` must be based on **string concatenation comparison**.

Key idea:

* When comparing two numbers `a` and `b`, compare `a+b` vs `b+a` (as strings).
* Example: `"9" + "34"` = `"934"` vs `"34" + "9"` = `"349"` → `"9"` should come before `"34"`.

Approach:

1. Convert all integers to strings.
2. Sort them using custom comparator:

   * If `a+b > b+a`, `a` should come first.
3. Join sorted array into result string.
4. Handle the edge case:

   * If the highest element is `"0"`, return `"0"` (means all are zeros).

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {

    public String largestNumber(int[] nums) {
        PriorityQueue<String> pq = new PriorityQueue<>((a, b) -> (b + a).compareTo(a + b));
        
        for(int n : nums) {
            pq.offer(Integer.toString(n));
        }
        
        StringBuilder sb = new StringBuilder();
        while(!pq.isEmpty()) {
            sb.append(pq.poll());
        }
        
        return (sb.charAt(0) == '0') ? "0" : sb.toString();
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n · k) — sorting with string compares
* **Space Complexity:** O(n · k) — storing string forms