## Jump Game

#### **Statement**

You are given an integer array `nums`, where each element represents the maximum number of steps you can move forward from that position. You always start at index 00 (the first element), and at each step, you may jump to any of the next positions within the range allowed by the current element’s value. Return TRUE if you can reach the last index, or FALSE otherwise.

---

#### ✔️ Constraints

- 1 ≤ `nums.length` ≤ 10^3
- 0 ≤ `nums[i]` ≤ 10^3

---

## 🎯 Intuition

This problem can be solved using a **Greedy approach**.

At any index, what matters is **how far we can reach**, not the exact path taken.
We keep track of the **farthest index reachable so far** while iterating through the array.

Key ideas:

* Start with `maxReach = 0`
* For each index `i`:

  * If `i > maxReach`, we cannot reach this position → return FALSE
  * Otherwise, update `maxReach = max(maxReach, i + nums[i])`
* If at any point `maxReach` reaches or exceeds the last index → return TRUE

This works because making the **locally optimal choice** (maximizing reach at each step) guarantees a **globally optimal result**.

---

## ✅ Java Solution

```java
public class JumpGame{
  public static boolean jumpGame(int[] nums) {
    int maxReach = 0;
    
    for(int i = 0; i < nums.length; i++) {
      // if at any index i, i > maxReach, it means we cannot go farther
      if(i > maxReach) return false;

      // maxReach is the farthest it can reach from all the paths
      maxReach = Math.max(maxReach, nums[i] + i);
    }
    
    return true;
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)