## 3Sum

Given an integer array `nums`, find and return **all unique triplets** `[nums[i], nums[j], nums[k]]` such that:

- `i ≠ j`, `i ≠ k`, and `j ≠ k`
- `nums[i] + nums[j] + nums[k] == 0`

The **order of triplets in the output does not matter**, but each distinct triplet should appear only once.

---

### Constraints

- `3 ≤ nums.length ≤ 500`
- `−10^3 ≤ nums[i] ≤ 10^3`

---
## Intuition

If we brute-force try every triplet, it will take **O(n³)** time — too slow for large input.

Key idea:  
If we **sort** the array, we can fix one number and then use **two pointers** to find the other two numbers that sum to `-fixed`.

---

## Path to Intuition

1. **Sort the array**
   - Makes it easier to avoid duplicates.
   - Allows two-pointer technique.

2. **Fix one element (`nums[i]`)**
   - For each `i`, the problem reduces to:  
     → Find two numbers with sum = `-nums[i]`

3. **Use two pointers**
   - Left pointer `l = i + 1`
   - Right pointer `r = n - 1`
   - Move them inward based on sum comparison.

4. **Avoid duplicates**
   - Skip same values for `i`, and also after finding a valid triplet.

This achieves optimal performance for constraints.

---

## Java Solution
#### 3Sum (Two-Pointer Approach)

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();

        // Once sorted, we must traverse the array
        for(int i = 0; i < nums.length; i++) {
            int a = nums[i]; // First element of the triplet

            // Base case: If the smallest number is already greater than 0,
            // sum can't be zero
            if(a > 0) break;

            // Skip duplicates for the first element
            if(i > 0 && a == nums[i - 1]) continue;

            // Execute two-pointer 2Sum
            twoSum(nums, i, res);
        }

        return res;
    }

    public void twoSum(int[] nums, int i, List<List<Integer>> res) {
        int l = i + 1, r = nums.length - 1;

        while(l < r) {
            int sum = nums[i] + nums[l] + nums[r];

            if(sum < 0) {
                l++;
            } else if(sum > 0) {
                r--;
            } else {
                res.add(Arrays.asList(nums[i], nums[l++], nums[r--]));
                
                // Skip duplicates for the second element
                while(l < r && nums[l] == nums[l - 1]) l++;
            }
        }
    }
}
```
- Time Complexity: O(n^2)
- Space Complexity: O(1)