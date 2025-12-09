## Count Pairs Whose Sum Is Less Than Target

#### 🧾 Problem Statement

Given a 0-indexed integer array `nums` of length `n` and an integer `target`,  
return the number of **distinct index pairs** `(i, j)` such that:

- `0 ≤ i < j < n`
    
- `nums[i] + nums[j] < target`
    

---

#### ✔️ Constraints

- `1 ≤ n ≤ 50`
    
- `-50 ≤ nums[i], target ≤ 50`
    

---

## 💡 Intuition

We want to count the number of valid pairs `(i, j)` where `i < j`  
and their sum is less than `target`.

#### Optimization

Sorting the array allows:

- If `nums[i] + nums[j] < target`  
    → Every element between `i` and `j` also forms a valid pair with `i`  
    → Add `(j - i)` pairs at once  
    → Move left pointer forward
    
- Else: sum is too big → move right pointer backward
    

This reduces checks from **O(n²)** brute force → **O(n log n)** sorting + **O(n)** scan.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution{
    public static int countPairs (List<Integer> nums, int target) {
      Collections.sort(nums);
      int l = 0, r = nums.size() - 1;
      
      int ans = 0;
      while(l < r) {
        if(nums.get(l) + nums.get(r) >= target) {
          r--;
        }
        else {
          ans += (r - l);
          l++;
        }
      }
      
      return ans;
    }
}
```
- **Time Complexity: O(nlogn)**
- **Space Complexity: O(1)**