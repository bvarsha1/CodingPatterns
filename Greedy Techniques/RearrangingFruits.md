## Rearranging Fruits

#### **Statement**

Given two 0-indexed integer arrays, `basket1` and `basket2`, representing the cost of each fruit in the basket. Each basket contains `n` fruits. Make the two baskets identical, i.e., both arrays should have the same costs.

To achieve this, perform the following operation as many times as necessary:

Select two indexes, `i` and `j`, and swap the fruit at index `i` in `basket1` with the fruit at index `j` in `basket2`.

The cost of this swap is `min(basket1[i], basket2[j])`.

The two baskets are considered identical if, after sorting the fruits by cost, both baskets contain exactly the same costs.

Return the minimum cost required to make the baskets identical, or `-1` if it is impossible.

---

#### ✔️ Constraints

* `basket1.length == basket2.length`
* 1 ≤ `basket1.length` ≤ 10^3
* 1 ≤ `basket1[i]`, `basket2[i]` ≤ 10^4

---

## 🎯 Intuition

This problem is **not about positions**, but about **balancing frequencies**.

Key observations:

* Since order doesn’t matter, we only care about how many times each fruit cost appears
* For the baskets to become identical:

  * The **total frequency** of each cost across both baskets must be **even**
  * Otherwise, it’s impossible
* We only need to swap the **extra fruits** from one basket to the other
* To minimize cost:

  * Swapping two large fruits directly is expensive
  * It may be cheaper to swap them **indirectly using the globally cheapest fruit**

Greedy strategy:

* Collect all excess fruit costs that need to be swapped
* Sort them
* For each required swap, the cost is:

```
min(fruitCost, 2 * minimumFruitCost)
```

This guarantees the minimum total swap cost.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static long minCost(int[] basket1, int[] basket2) {
        Map<Integer, Integer> freq = new HashMap<>();
        int minVal = Integer.MAX_VALUE;

        for (int x : basket1) {
            freq.put(x, freq.getOrDefault(x, 0) + 1);
            minVal = Math.min(minVal, x);
        }

        for (int x : basket2) {
            freq.put(x, freq.getOrDefault(x, 0) - 1);
            minVal = Math.min(minVal, x);
        }

        List<Integer> extra = new ArrayList<>();

        for (int val : freq.keySet()) {
            int diff = freq.get(val);
            if ((diff & 1) != 0) {
                return -1;
            }

            for (int i = 0; i < Math.abs(diff) / 2; i++) {
                extra.add(val);
            }
        }

        Collections.sort(extra);

        long cost = 0;
        int swaps = extra.size() / 2;
        for (int i = 0; i < swaps; i++) {
            cost += Math.min(extra.get(i), 2L * minVal);
        }

        return cost;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n)
* **Space Complexity:** O(n)
