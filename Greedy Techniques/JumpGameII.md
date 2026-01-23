## Jump Game II

#### **Statement**

In a single player jump game, the player starts at one end of a series of squares and aims to reach the last square.

At each turn, the player can take up to *s* steps toward the last square, where *s* is the value of the current square.

For example, if the value of the current square is 3, the player can take either 3 steps, 2 steps, or 1 step in the direction of the last square. The player cannot move in the opposite direction, away from the last square.

You’ve been provided with the nums integer array, representing the series of squares.

You’re initially positioned at the first index of the array. Find the minimum number of jumps needed to reach the last index of the array.

You may assume that you can always reach the last index.

---

#### ✔️ Constraints

* 1 ≤ `nums.length` ≤ 10<sup>3</sup>
* 0 ≤ `nums[i]` ≤ 10<sup>3</sup>

---

## 🎯 Intuition

This problem can be solved using a **Greedy Sliding Window** approach.

Key insight:

* Every time you jump, you want to choose a landing spot that allows the **maximum reach** for the next jump.
* Think of it as expanding layers:

  * `currentEnd` = boundary of current jump
  * `farthest` = the farthest we can reach during this layer
* When the current index hits `currentEnd`, it means:

  * you must make a jump
  * update `currentEnd = farthest`

This ensures **minimum jumps**, because we only jump when we must.

---

## 🧠 Greedy Approach Breakdown

1. Initialize:

   * `jumps = 0`
   * `currentEnd = 0`
   * `farthest = 0`
2. Iterate `i` from 0 → `n - 2` (no need to jump from last position)
3. Update `farthest = max(farthest, i + nums[i])`
4. If `i == currentEnd`:

   * increment `jumps`
   * update `currentEnd = farthest`

---

## ✅ Java Solution

```java
class Solution {
    public int jump(int[] nums) {
        int jumps = 0;
        int currentEnd = 0;
        int farthest = 0;

        for (int i = 0; i < nums.length - 1; i++) {
            farthest = Math.max(farthest, i + nums[i]);
            
            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;
            }
        }
        return jumps;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)