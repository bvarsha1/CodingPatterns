## **Partition Equal Subset Sum**

#### Statement

Given a non-empty array of positive integers, determine if the array can be divided into two subsets so that the sum of both subsets is equal.

---

#### ✔️ Constraints

* 1 ≤ `nums.length` ≤ 200
* 1 ≤ `nums[i]` ≤ 100

---

## 🎯 **Intuition**

If the array can be split into two equal-sum subsets:

Let `total = sum(nums)`

For equal split:

* `total` **must be even**, otherwise immediately `FALSE`.

So we only need to check:

> Can we pick a subset whose sum = `total / 2`?

This is now a **Subset Sum Problem**:

```
Does a subset exist whose sum = total/2?
```

This maps perfectly to a **0/1 knapsack** where:

* capacity = `total/2`
* each element = weight value = nums[i]

If such a subset exists → remaining elements form the other subset.

### 🧠 **Approach**

1. Compute `sum = total of nums`
2. If `sum` is odd → return `false`
3. Target becomes `sum/2`
4. Use DP to check if subset-sum = target exists

#### 🧩 **Bottom-Up DP Solution (Space Optimized)**

We use a boolean DP array:

```
dp[x] = true means: a subset with sum x exists.
```

Initialize:

```
dp[0] = true (empty subset always makes sum 0)
```

Then for each number `num`, update dp backwards:

```
for (w = target → num):
    dp[w] |= dp[w - num]
```

If `dp[target] == true` → partition exists.

---

## ✅ **Java Code**

#### 0/1 Knapsack 1D DP solution

```java
class PartitionEqualSubsetSum {
    public static boolean canPartition(int[] nums) {
        int total = 0;
        for (int num : nums) total += num;

        if (total % 2 != 0) return false; // can't split odd total

        int target = total / 2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;

        for (int num : nums) {
            for (int w = target; w >= num; w--) {
                if (dp[w - num]) {
                    dp[w] = true;
                }
            }
        }

        return dp[target];
    }
}
```

#### Subset Sum Solution

```java
import java.util.*;
public class PartitionEqualSum{
   public static boolean canPartitionArray(int[] arr) {
      int sum = 0;
      
      for(int n : arr) sum += n;
      if(sum % 2 != 0) return false;
      
      int target = sum / 2;
      HashSet<Integer> dp = new HashSet<>();
      dp.add(0);
      
      for(int i = 0; i < arr.length; i++) {
        HashSet<Integer> newDP = new HashSet<>();
        for(int n : dp) {
          if(!dp.contains(n + arr[i])) newDP.add(n + arr[i]);
        }
        dp.addAll(newDP);
      }
      
      if(dp.contains(target)) return true;
      
      return false;
   }
}
```

#### ⏱ **Complexity**

* **Time Complexity:** `O(n × sum)` where `sum = total/2`
* **Space Complexity:** `O(sum)`