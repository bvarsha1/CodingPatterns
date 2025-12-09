## Append Characters to String to Make Subsequence

#### Statement

You’re given two strings, source and target, made up of lowercase English letters. Your task is to determine the minimum number of characters that must be appended to the end of the source so that the target becomes a subsequence of the resulting string.

Note: A subsequence is formed by deleting zero or more characters from a string without changing the order of the remaining characters.

---

#### ✔️ Constraints

* `1 ≤ source.length, target.length ≤ 10³`
* `source and target consist only of lowercase English letters.`

---

## 🎯 Intuition

We want `target` to become a subsequence of `source`.
So — we try to match characters of `target` while iterating through `source`:

* Use two pointers `i` (for `source`) and `j` (for `target`)
* Every time characters match → move both forward
* Otherwise → only move in `source`

Once we finish scanning `source`:

* If `j` has not reached the end of `target`,
  → the remaining characters `target[j ... end]` must be appended

So the result = number of unmatched characters in `target`.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public int appendCharacters(String source, String target) {
        int m = source.length(), n = target.length();
        
        int i = 0, j = 0;
        // try to match characters of target within source
        while (i < m && j < n) {
            if (source.charAt(i) == target.charAt(j)) {
                j++;
            }
            i++;
        }
        
        // characters left in target must be appended
        return n - j;
    }
}
```
#### ⏱ Complexity

* **Time Complexity:** O(m + n)
* **Space Complexity:** O(1)