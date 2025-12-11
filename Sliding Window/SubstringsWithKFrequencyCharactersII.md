## Count Substrings With K-Frequency Characters II

#### **Statement**

You are given a string `s` and an integer `k`.
Your task is to **return the total number of substrings where at least one character appears at least `k` times**.

> A substring is a contiguous sequence within a string.
> Example: `"edu"` is a substring of `"educative"`.

---

#### ✔️ Constraints

* `1 ≤ s.length ≤ 3 × 10³`
* `1 ≤ k ≤ s.length`
* string consists of lowercase English letters

---

## 🎯 Intuition

This version uses a **true sliding window**, unlike the O(n²) expanding approach.

### Key insight

When a window `[left … right]` contains **any** character with frequency ≥ `k`,
**every substring starting at `left` and ending at any index ≥ right is valid**, i.e.:

```
[left..right], [left..right+1], ..., [left..n-1]
```

So once the window satisfies the condition, we can immediately add:

```
(n - right)
```

to the answer.

### How the sliding window works

1. Expand the window by moving `right`.
2. Update the frequency of the added character.
3. When a character reaches frequency `k`, increment a counter `charsWithK`.
4. Now shrink from the left:

   * Each shrink produces valid substrings, so add `(n - right)` to the total.
   * Reduce the frequency of `s[left]`.
   * If that frequency drops below `k`, decrement `charsWithK`.
   * Move `left` forward.

Because each pointer (`left`, `right`) moves only forward,
this gives a **true O(n)** two-pointer solution.

---

## ✅ Java Solution (Cleaned & Simplified)

```java
public class Solution {

    public long numberOfSubstrings(String s, int k) {
        int n = s.length();
        int[] freq = new int[26];
        int charsWithK = 0;
        long total = 0L;
        int left = 0;

        for (int right = 0; right < n; right++) {

            int idx = s.charAt(right) - 'a';
            freq[idx]++;
            if (freq[idx] == k) charsWithK++;

            while (charsWithK > 0) {
                total += (n - right);

                int leftIdx = s.charAt(left) - 'a';
                if (freq[leftIdx] == k) charsWithK--;
                freq[leftIdx]--;
                left++;
            }
        }

        return total;
    }
}
```

## ⏱️ Complexity

* **Time Complexity:** O(n) — each pointer moves forward at most `n` times.
* **Space Complexity:** O(1) — frequency array of size 26.
