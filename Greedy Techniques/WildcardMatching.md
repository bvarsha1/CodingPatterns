## Wildcard Matching

#### **Statement**

Given an input string, `s`, and a pattern string, `p`, implement wildcard pattern matching that determines if the pattern matches the entire input string.

The pattern supports two special wildcard characters:

- `'?'`: Matches exactly one arbitrary character.

- `'*'`: Matches any sequence of characters (including zero characters).

The match must be complete, meaning the pattern should cover the entire input string, not just a part of it.

Return TRUE if the pattern matches the whole string; otherwise, return FALSE.

---

#### ✔️ Constraints

* `0 ≤ s.length, p.length ≤ 2000`
* `s` contains only lowercase English letters.
* `p` contains only lowercase English letters, `'?'` or `'*'`.

---

## 🎯 Intuition

This problem can be solved via **Dynamic Programming**, but there is a cleaner **Greedy + Backtracking** approach that runs in linear time.

Important observations:

* `'?'` is deterministic — matches exactly **one** character.
* Literal characters must match exactly.
* `'*'` is the only wildcard with flexibility — it can match **any sequence** (including empty).

So the greedy idea becomes:

1. Scan both strings with two pointers.
2. When we see `'*'`, remember its position because we may need to **backtrack** later.
3. If characters mismatch and we have seen `'*'` before, expand that `'*'` to cover more characters from `s`.

---

## ⚙️ Greedy Approach

We maintain four state variables:

| Variable     | Meaning                                            |
| ------------ | -------------------------------------------------- |
| `i`          | Pointer for `s`                                    |
| `j`          | Pointer for `p`                                    |
| `starIndex`  | Most recent index of `'*'` in `p`                  |
| `matchIndex` | Position in `s` where matching resumed after `'*'` |

Algorithm behavior:

1. If characters match or pattern has `'?'` → move both pointers.

2. If pattern has `'*'` → store `starIndex = j`, `matchIndex = i`, and move `j`.

3. If mismatch and `starIndex != -1`:

   → backtrack: set `j = starIndex + 1` and increase `matchIndex`, then set `i = matchIndex`.

4. If mismatch and no previous `'*'` exists → return FALSE.

Finally, ensure remaining characters in `p` are all `'*'`.

This works in **O(n + m)** time and **O(1)** space.

---

## ✅ Java Solution (Greedy)

```java
public class Solution {
    public boolean isMatch(String s, String p) {
        int i = 0, j = 0;
        int starIdx = -1, matchIdx = -1;
        while(i < s.length()) {
            if(j < p.length() && (p.charAt(j) == '?' || s.charAt(i) == p.charAt(j))) {
                i++;
                j++;
            } else if(j < p.length() && p.charAt(j) == '*') {
                starIdx = j;
                matchIdx = i;
                j++;
            } else if(starIdx != -1) {
                j = starIdx + 1;
                i = ++matchIdx;
            } else {
                return false;
            }
        }
        
        while(j < p.length() && p.charAt(j) == '*') {
            j++;
        }
        
        return j == p.length();
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n + m)
* **Space Complexity:** O(1)
