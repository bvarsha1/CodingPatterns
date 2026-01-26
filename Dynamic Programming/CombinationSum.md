## Combination Sum

#### Statement

Given an array of distinct integers, `nums`, and an integer, `target`, return a list of all unique combinations of `nums` where the chosen numbers sum up to the `target`. The combinations may be returned in any order.

An integer from `nums` may be chosen an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen integers is different.

---

#### ✔️ Constraints

* 1 ≤ `nums.length` ≤ 30
* 2 ≤ `nums[i]` ≤ 40
* 1 ≤ `target` ≤ 40
* All integers of `nums` are unique.

---

## 🎯 Intuition

We need to form sums equal to `target` using `nums` where:

* Each number can be taken **unlimited times**
* Order of picking doesn't matter (i.e., no permutations)

This maps directly to an **Unbounded Knapsack** variant where:

```
capacity = target
items = nums[]
each item weight = value = nums[i]
```

But unlike classical knapsack, we must **return actual combinations**, not just counts.

There are multiple approaches:

### 🧠 Approach 1: Backtracking (DFS)

Classic solution:

* Try including `nums[i]` and recurse with same index
* Try skipping it and move to next index
* Stop when sum matches or exceeds `target`

This ensures uniqueness because indices never decrease.

### 🧠 Approach 2: Dynamic Programming (Bottom-Up) — Build Combinations

DP formulation:

```
dp[s] = all combinations that sum to s
```

Initialize:

```
dp[0] = [[]]
```

Transition:

For each `num` in `nums`, for each `sum` from `num` → `target`, extend combinations from `dp[sum - num]`.

To avoid permutations like `[2,3]` and `[3,2]`, we enforce combinations to be **non-decreasing**.

### 🧩 Why DP Works Here

DP gradually builds all valid sums <= target.

Each state reuses previously computed subsets, propagating combinations forward.

This avoids recursive overhead and gives explicit tabulation of states.

#### 🧪 Example

nums = `[2,3,6,7]`, target = `7`

DP builds:

```
dp[2] = [[2]]
dp[3] = [[3]]
dp[4] = [[2,2]]
dp[5] = [[2,3]]
dp[6] = [[2,2,2], [3,3], [6]]
dp[7] = [[2,2,3], [7]]
```

### Backtracking Solution (Reference)

```java
import java.util.*;

public class Solution {
    public static List<List<Integer>> combinationSum(int[] nums, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        combinationSumRec(nums, 0, target, new ArrayList<Integer>(), ans);

        return ans;
    }
  
    public static void combinationSumRec(int[] nums, int i, int t, List<Integer> curr, List<List<Integer>> ans) {
        // base cases 
        if(i == nums.length) {
            return;
        }

        if(t == 0) {
            ans.add(new ArrayList<>(curr));
            return;
        }

        // recurrence
        // including the current integer
        if(t - nums[i] >= 0) {
            curr.add(nums[i]);
            combinationSumRec(nums, i, t - nums[i], curr, ans);
            curr.remove(curr.size() - 1);
        }
        // excluding the current integer, and moving to next
        combinationSumRec(nums, i + 1, t, curr, ans);
    }
}
```

#### ⏱ Complexity

* Time: exponential in worst case (due to combination enumeration)
* Space: O(target) recursion depth

<br>

### DP Solution (Bottom-Up : 0/1 Knapsack Inspired)

```java
import java.util.*;

public class CombinationSum {
  public static List<List<Integer>> combinationSum(int[] nums, int target) {
    List<List<List<Integer>>> dp = new ArrayList<>();
    for(int i = 0; i <= target; i++) dp.add(new ArrayList<>());
    dp.get(0).add(new ArrayList<>());
    
    for(int n : nums) {
      for(int w = n; w <= target; w++) {
        for(List<Integer> combo : dp.get(w - n)) {
          List<Integer> newCombo = new ArrayList<>(combo);
          newCombo.add(n);
          dp.get(w).add(newCombo);
        }
      }
    }
    
    return dp.get(target);
  }
}
```

#### ⏱ Complexity

Let:

* `n = nums.length`
* `T = target`
* `K = average number of valid combinations per sum`

Then:

* **Time Complexity:** `O(n × T × K)`
* **Space Complexity:** `O(T × K)`

<br>
<br>

### When to Prefer Each Method

| Method              | When to Use                                 |
| ------------------- | ------------------------------------------- |
| Backtracking (DFS)  | Small target, few combinations              |
| DP (Storing combos) | Need structured DP or avoiding recursion    |
| DP Count-Only       | Only need number of ways, not actual combos |
