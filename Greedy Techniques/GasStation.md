## Gas Station

#### **Statement**

There are `n` gas stations along a circular route, where the amount of gas at the *i*th station is `gas[i]`.

We have a car with an unlimited gas tank, and it costs `cost[i]` of gas to travel from the *i<sup>th</sup>* station to the next *(i + 1)<sup>th</sup>* station. We begin the journey with an empty tank at one of the gas stations.

Find the index of the gas station in the integer array `gas` such that if we start from that index we may return to the same index by traversing through all the elements, collecting `gas[i]` and consuming `cost[i]`.

- If it is not possible, return `-1`.

- If there exists such index, it is guaranteed to be unique.

---

#### ✔️ Constraints

* `gas.length == cost.length`
* 1 ≤ `gas.length`, `cost.length` ≤ 10^3
* 0 ≤ `gas[i]`, `cost[i]` ≤ 10^3

---

## 🎯 Intuition

This problem can be solved using a **Greedy approach**.

Key observations:

* If the **total gas available** is less than the **total cost**, completing the circuit is impossible.
* If a solution exists, the starting index is **unique**.
* If we fail to reach station `i + 1` from a chosen start, then **any station between the start and `i` cannot be a valid starting point**.

Approach:

* Traverse all stations while maintaining:

  * `totalTank`: total gas balance across all stations
  * `currTank`: gas balance for the current candidate start
* If `currTank` drops below zero, reset the start to the next station and reset `currTank`

This works because we discard impossible starting points in one pass.

---

## ✅ Java Solution

```java
public class Solution {
    public static int canCompleteCircuit(int[] gas, int[] cost) {
        int totalTank = 0;
        int currTank = 0;
        int start = 0;

        for (int i = 0; i < gas.length; i++) {
            int diff = gas[i] - cost[i];
            totalTank += diff;
            currTank += diff;

            if (currTank < 0) {
                start = i + 1;
                currTank = 0;
            }
        }

        return totalTank >= 0 ? start : -1;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)