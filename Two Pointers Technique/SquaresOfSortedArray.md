## Squares of a Sorted Array

#### Statement

You are given an integer array, `nums`, sorted in non-decreasing order. Your task is to return a new array containing the squares of each number, also sorted in non-decreasing order.

---

#### ✔️ Constraints

* `1 ≤ nums.length ≤ 10³`
* `−10⁴ ≤ nums[i] ≤ 10⁴`
* `nums is sorted in non-decreasing order.`

---

## 🎯 Intuition

The negative values become positive when squared — and could become the largest numbers.
So the smallest/largest squares must come from the **edges** of the array.

Example:
`nums = [-7, -3, 2, 3, 11]`
Squares → `[49, 9, 4, 9, 121]` (not sorted yet)

We use **two pointers**:

| Pointer | Starts at | Purpose                                           |
| ------- | --------- | ------------------------------------------------- |
| `s`     | 0         | Track left side (possibly large negative squares) |
| `e`     | n-1       | Track right side (positive squares)               |

We assign the **larger** square to the **end** of the result array and move inward.

This ensures sorted order in one pass.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        int s = 0, e = n - 1;
        int idx = n - 1;

        while (s <= e && idx >= 0) {
            if (square(nums[s]) >= square(nums[e])) {
                result[idx--] = square(nums[s++]);
            } else {
                result[idx--] = square(nums[e--]);
            }
        }
        return result;
    }

    public static int square(int x) {
        return x * x;
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)