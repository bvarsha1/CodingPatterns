## Two City Scheduling

#### **Statement**

A recruiter plans to hire `n` people and conducts their interviews at two different locations of the company. He evaluates the cost of inviting candidates to both these locations. The plan is to invite 50% at one location, and the rest at the other location, keeping costs to a minimum.

We are given an array, `costs`, where `costs[i] = [aCost_i, bCost_i]`, the cost of inviting the *i*th person to City **A** is `aCost_i`, and the cost of inviting the same person to City **B** is `bCost_i`.

You need to determine the minimum cost to invite all the candidates for the interview such that exactly `n/2` people are invited in each city.

---

#### ✔️ Constraints

* 2 ≤ `costs.length` ≤ 100
* `costs.length` is even
* 1 ≤ `aCost_i`, `bCost_i` ≤ 1000

---

## 🎯 Intuition

This problem is a classic **Greedy optimization** problem.

Key idea:

* If we send everyone to City A, the base cost would be the sum of all `aCost`
* Sending a person to City B instead of City A changes the cost by
  **`bCost - aCost`**
* To minimize total cost, we should send the people with the **largest savings** (most negative difference) to City B

Approach:

1. Sort people by the difference `(aCost - bCost)`
2. Send the first `n/2` people to City **A**
3. Send the remaining `n/2` people to City **B**

This ensures exactly half the people go to each city at minimum total cost.

---

## ✅ Java Solution

```java
import java.util.Arrays;

public class Solution {
    public static int twoCitySchedCost(int[][] costs) {
        Arrays.sort(costs, (a, b) -> (a[0] - a[1]) - (b[0] - b[1]));

        int n = costs.length / 2;
        int totalCost = 0;

        for (int i = 0; i < costs.length; i++) {
            if (i < n) {
                totalCost += costs[i][0]; // City A
            } else {
                totalCost += costs[i][1]; // City B
            }
        }

        return totalCost;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n) — due to sorting
* **Space Complexity:** O(1) — ignoring sort space