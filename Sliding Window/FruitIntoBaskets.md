## Fruit Into Baskets

#### **Statement**

While visiting a farm of fruits, you have been given a row of fruits represented by an integer array, `fruits`, where `fruits[i]` is the type of fruit the
iᵗʰ tree produces. You have to collect fruits, but there are some rules that you must follow while collecting fruits:

- You are given only two baskets, each capable of holding an unlimited quantity of a single type of fruit.

- You can start collecting from any tree but must collect exactly one fruit from each tree (including the starting tree) while moving to the right.

- You must stop while encountering a tree with a fruit type that cannot fit into any of your baskets.

Return the maximum number of fruits you can collect following the above rules for the given array of fruits.

---

#### ✔️ Constraints

* `1 ≤ fruits.length ≤ 10^3`
* `0 ≤ fruits[i] < fruits.length`

---

## 🎯 Intuition

We need the **longest contiguous subarray** that contains **at most two distinct fruit types**.

This is a classic **sliding window** problem where we expand the window to include new trees (fruits) and **shrink** it when the number of distinct fruit types exceeds **2**.

#### 🔹 Key Idea

* Maintain a window `[i … j]` and a `HashMap<fruitType, count>` to store counts of fruit types inside the window.
* As we move `j` (right end) forward, increment the count for `fruits[j]`.
* If the map size becomes greater than `2`, move the left pointer `i` forward, decrementing counts and removing fruit types with zero count, until the map size is ≤ 2 again.
* At each step, the window length `j - i + 1` is a candidate answer — keep the maximum.

This gives each element at most one insert and one remove → **O(n)** time. The hashmap never holds more than 2 keys, so extra space is **O(1)**.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public int totalFruit(int[] fruits) {
      HashMap<Integer, Integer> map = new HashMap<>();
      int total = 0;
      int i = 0;
      for(int j = 0; j < fruits.length; j++) {
        int fruit = fruits[j];
        map.put(fruit, map.getOrDefault(fruit, 0) + 1);
        
        // contraction
        while(map.size() > 2) {
          if(map.get(fruits[i]) == 1) {
            map.remove(fruits[i]);
          } else {
            map.put(fruits[i], map.get(fruits[i]) - 1);
          }
          i++;
        }
        
        // window validation
        total = Math.max(total, j - i + 1);
      }
      
      return total;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n) — each index enters and leaves the window at most once.
* **Space Complexity:** O(1) — hashmap size ≤ 2 (constant extra space).
