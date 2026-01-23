## Minimum Replacements to Sort the Array

#### **Statement**

You are given a 0-indexed integer array `nums`. You are allowed to perform the following operation any number of times:

Select any element in the array and replace it with two positive integers whose sum is equal to the selected element.

For example, if the array is `nums = [5, 6, 7]`, you can choose the element `6` and replace it with `2` and `4`, resulting in a new array `[5, 2, 4, 7]`.

Your goal is to make the array sorted in non-decreasing order using the minimum number of operations.

Return the minimum number of operations required to achieve this.

---

#### ✔️ Constraints

* 1 ≤ `nums.length` ≤ 10<sup>3</sup>
* 1 ≤ `nums[i]` ≤ 10<sup>5</sup>

---

## 🎯 Intuition

To make the array non-decreasing, we only need to worry about elements that violate the rule when moving from **right to left**.

Key idea:

* Maintain a variable `prev` initialized as the last element
* Traverse from right to left:

  * If `nums[i] <= prev`, simply set `prev = nums[i]`
  * If `nums[i] > prev`, we must split `nums[i]` into smaller parts so that all resulting parts are ≤ `prev`

How to split:

If we want `nums[i]` to be split into `k` parts such that each part is ≤ `prev`, then:

```
k = ceil(nums[i] / prev)
```

Every split adds `(k - 1)` operations.

After splitting, the new `prev` becomes:

```
prev = nums[i] / k  (integer floor)
```

This greedy strategy ensures minimal replacements.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution{
   public static long minimumReplacement(int[] ratings) {
      int n = ratings.length;
      
      int prev = ratings[n - 1];
      long ops = 0L;
      for(int i = n - 2; i >= 0; i--) {
         if(ratings[i] <= prev) {
            prev = ratings[i];
         } else {
            int currOps = (ratings[i] + prev - 1) / prev;
            ops += (currOps - 1);
            prev = ratings[i] / currOps;
         }
      }
      
      return ops;
   }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)