## Contains Duplicate II

#### **Statement**

You are given an integer array, nums, and an integer k. Determine whether two distinct indices, i and j, are in the array, such that nums[i] == nums[j] and the absolute difference between i and j is at most k. Return TRUE if such indices exist; otherwise, return FALSE.

---

#### ✔️ Constraints

* `1 ≤ nums.length ≤ 10^3`
* `-10^3 ≤ nums[i] ≤ 10^3`
* `0 ≤ k ≤ 10^4`

---

## 🎯 Intuition

We need to know whether the same value appears **twice within a window of size k+1** (distance ≤ k).
A sliding-window set keeps the last `k` (actually `k` previous) elements:

* As we iterate through the array, maintain a **hash set** of the most recent up-to-`k` values (the current window).
* For each new element `nums[j]`:

  * If it's already in the set → we found a duplicate within distance `≤ k` → **return true**.
  * Otherwise add it to the set.
  * If the set size grows larger than `k`, remove the element that fell out of the window (`nums[j-k]`).
* If we finish the loop without finding any duplicate in a window of size `k+1`, return **false**.

This gives **O(1)** average-time membership checks and ensures we only store at most `k` elements.

---

## ✅ Java Solution

```java
import java.util.*;
public class Solution {
  public static boolean containsNearbyDuplicate(int[] nums, int k) {
    Set<Integer> seen = new HashSet<>();
    for (int j = 0; j < nums.length; j++) {
      if (seen.contains(nums[j])) return true;
      seen.add(nums[j]);
      if (seen.size() > k) seen.remove(nums[j - k]);
    }
    return false;
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n) — each element is inserted/removed/checked at most once (average case for HashSet).
* **Space Complexity:** O(min(n, k)) — the set holds at most `k` elements.
