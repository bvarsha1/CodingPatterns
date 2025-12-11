## Subarrays with K Different Integers

#### **Statement**

You are given an integer array, `nums`, and an integer `k`.
Your task is to return the number of **good subarrays** of `nums`.

> A **good subarray** is a contiguous subarray that contains **exactly `k` distinct integers**.

> For example, in the array `[1, 2, 3, 1, 2]`, the subarray `[1, 2, 3]` contains **3 distinct integers**: `1`, `2`, and `3`.

---

#### ✔️ Constraints

* `1 ≤ nums.length ≤ 2 * 10^4`
* `1 ≤ nums[i], k ≤ nums.length`

---

## 🎯 Intuition

A classical approach to find subarrays with **exactly** `k` distinct integers is to compute:

```
#subarrays with exactly k distinct 
= (#subarrays with at most k distinct)  
  - (#subarrays with at most k−1 distinct)
```

The “at most K distinct” version can be solved using a sliding window + frequency hashmap.
As we expand the window, if distinct integers exceed `k`, we contract from the left until valid again.
The total number of valid subarrays ending at each index gives the count.

This approach ensures a clean O(n) solution for each “at most K” query.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution{
    public static int subarraysWithKDistinct(int[] nums, int k) {
        return subarraysWithAtMostK(nums, k) - subarraysWithAtMostK(nums, k - 1);
    }
    
    public static int subarraysWithAtMostK(int[] nums, int k) {
        HashMap<Integer, Integer> freq = new HashMap<>();
        
        int count = 0;
        int i = 0;
        for(int j = 0; j < nums.length; j++) {
            freq.put(nums[j], freq.getOrDefault(nums[j], 0) + 1);
            
            // contraction
            while(freq.size() > k) {
                if(freq.get(nums[i]) == 1) {
                    freq.remove(nums[i]);
                } else {
                    freq.put(nums[i], freq.get(nums[i]) - 1);
                }
                i++;
            }
            
            count += j - i + 1;
        }
        
        return count;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n) for each “at most K” computation → overall O(n).
* **Space Complexity:** O(k) for the frequency map.

---

> **Note:** 
> * Also refer NeetCode approach on Sliding Window + 3 * ptr technique: https://www.youtube.com/watch?v=etI6HqWVa8U