## Remove Element

#### Statement

You are given an integer array, `nums`, and an integer, `val`.
Your task is to remove **all occurrences** of `val` from `nums` **in-place** (⚠️ no extra space).

After modification:

* The **first `k` elements** of `nums` must be values **not equal** to `val`
* `k` = count of remaining valid elements
* Elements after index `k-1` do **not** matter

Return **`k`**.

---

#### ✔️ Constraints

* `0 ≤ nums.length ≤ 100`
* `0 ≤ nums[i] ≤ 50`
* `0 ≤ val ≤ 100`

---

## 🎯 Intuition

Use a **write pointer** `j` that keeps track of the next valid position.

| Pointer | Meaning                                    |
| ------- | ------------------------------------------ |
| `i`     | scans entire array                         |
| `j`     | stores valid elements (not equal to `val`) |

Whenever `nums[i] != val`, copy it to `nums[j]` and move `j` forward.

This ensures:

* In-place update ✔️
* Modified array’s first `k` positions contain required values ✔️
* Minimal operations ✔️

---

## ✅ Java Solution

```java
public class Solution {
    public int removeElement(int[] nums, int val) {
        int j = 0;
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] != val) {
                nums[j] = nums[i];
                j++;
            }
        }
        return j;
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)