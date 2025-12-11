## Frequency of the Most Frequent Element

#### **Statement**

You are given an integer array, nums, and an integer k, representing the maximum number of operations you can perform. In each operation, you may select any index in nums and increment its value by 1.

Your task is to determine the maximum possible frequency of a single element in the final array after performing at most k operations. You can choose to increase any elements in a way that results in one particular element appearing as often as possible (within k operations). For example, if nums = [2, 2, 3] and k = 4, you can increment the first and the second element, 2, once to match the third element, 3, achieving a maximum frequency of 3.

Return the highest frequency that can be achieved for any element in nums after at most k operations.

> The frequency of an element is the number of times it appears in an array.

---

#### ✔️ Constraints

* `1 ≤ nums.length ≤ 10^3`
* `1 ≤ nums[i] ≤ 10^3`
* `1 ≤ k ≤ 10^3`

---

## 🎯 Intuition

To maximize the frequency of some value `x` after at most `k` increments, it's optimal to **raise some smaller elements up to a larger element** (not lower the larger elements). If we sort `nums`, then for any position `r` (the target value `nums[r]`), we want the longest contiguous block ending at `r` (i.e., a window `[l..r]`) that can be increased so every element in the window equals `nums[r]` within `k` total increments.

For a sorted window `[l..r]`, the total increments required = `nums[r] * (r - l + 1) - sum(nums[l..r])`.
So we can use a **sliding window with two pointers** and maintain the running sum of the window. Expand `r` and, while required operations exceed `k`, move `l` forward. The maximum window length found is the answer.

This yields an efficient O(n log n) overall (sorting + linear window).

---

## ✅ Java Solution

```java
import java.util.Arrays;

public class Solution {
    public static int maxFrequency(int[] nums, int k) {
        Arrays.sort(nums);              // sort the array
        int n = nums.length;
        long windowSum = 0;            // use long to avoid overflow when multiplying
        int l = 0;
        int maxFreq = 1;

        for (int r = 0; r < n; r++) {
            windowSum += nums[r];

            // required increments to make all values in [l..r] equal to nums[r]:
            // nums[r] * (r - l + 1) - windowSum
            while ((long) nums[r] * (r - l + 1) - windowSum > k) {
                windowSum -= nums[l];
                l++;
            }

            maxFreq = Math.max(maxFreq, r - l + 1);
        }

        return maxFreq;
    }

    // Example usage:
    public static void main(String[] args) {
        int[] nums = {1,2,4};
        int k = 5;
        System.out.println(maxFrequency(nums, k)); // expected 3
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n) — dominated by sorting; the two-pointer window is O(n).
* **Space Complexity:** O(1) (ignoring input sort space), or O(n) if sort implementation uses extra memory.
