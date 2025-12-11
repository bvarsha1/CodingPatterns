## **Maximum Average Subarray I**

#### **Statement**

Given an array of integers nums, and an integer k, return the maximum average of a contiguous subarray of length k.

---

#### ✔️ **Constraints**

* `1 ≤ k ≤ nums.length ≤ 10^5`
* `−10^4 ≤ nums[i] ≤ 10^4`

---

## 🎯 **Intuition**

We need the **maximum average** of any contiguous subarray of fixed size `k`.

Key simplification:

👉 **Max average subarray of size k = subarray with the maximum sum of size k.**

Why?
Because `average = sum / k` and `k` is constant, so maximizing sum maximizes average.

#### 🔹 Sliding Window Approach (Size k)

1. First compute the sum of the **first k elements** → initial window.
2. Then slide the window across the array:

   * Add the new element entering the window
   * Subtract the old element leaving the window
3. Track the **maximum window sum** seen so far.
4. Return `maxSum / k`.

This approach runs in **O(n)** and keeps memory usage constant.

---

## ✅ **Java Solution**

```java
public class Solution {
  public static double findMaxAverage(int[] nums, int k) {
    int maxSum = 0, currSum = 0;
    
    for (int i = 0; i < k; i++) {
      currSum += nums[i];
      maxSum = currSum;
    }
    
    for (int i = k; i < nums.length; i++) {
      currSum = currSum + nums[i] - nums[i - k];
      maxSum = Math.max(maxSum, currSum);
    }
    
    return (double) maxSum / k;
  }
}
```

#### ⏱️ **Complexity**

* **Time Complexity:** O(n)
  Only one pass after the initial window.
* **Space Complexity:** O(1)
  Sliding window uses constant extra space.
