## Minimum Window Subsequence

#### **Statement**

Given two strings, s1 and s2, find and return the shortest substring of s1 in which all the characters of s2 appear in the same order, but not necessarily next to each other (i.e., s2 should be a subsequence of the substring).

If no such substring exists, return an empty string "". If there are multiple shortest substrings, return the one that appears first in s1 (i.e., with the left-most starting index).

Note: A substring is a contiguous sequence of characters within a string. A subsequence is a sequence of characters that can be derived from a string by deleting some characters without changing the order of the remaining characters. For example, “edu” is a substring and “cave” is a subsequence of “educative.”

---

#### ✔️ Constraints

- `1 ≤ s1.length ≤ 2×10³`
- `1 ≤ s2.length ≤ 100`
- s1 and s2 consist of uppercase and lowercase English letters.

---

## 🎯 Intuition

Finding a substring is normally about matching patterns contiguously — but here, **s2 must appear as a subsequence inside a substring of s1**, not as a substring itself.

This changes the approach entirely.

#### ❌ Brute Force

Try all substrings of s1 and check if s2 is a subsequence → **O(n² · m)** → too slow for n = 2000.

---

#### ✔️ Forward + Backward Two-Pass Technique

The key insight:
We don’t need to check every substring — only those that actually complete the subsequence.

#### 🔹 Step 1: Forward Scan

Walk through s1 and try to match s2 character by character.

Whenever we finish matching all of s2, we know:

➡ A valid window **ENDS here**.

But it's not guaranteed shortest, because extra characters might be at the start.

---

#### 🔹 Step 2: Backward Scan

From that end index, walk backwards to find the **earliest possible start** that still matches all characters of s2 (in reverse order).

This shrinks the window to the **minimum possible length** for this end position.

---

#### 🔹 Why this works

Every time we finish matching s2 forward, we immediately minimize the window backward.

Thus each window examined is guaranteed to:

✔ contain s2
✔ be the shortest for that end position
✔ be checked only once

This keeps the complexity manageable.

---

#### 🚀 Example Intuition (Quick)

If s1 = `"abcdebdde"` and s2 = `"bde"`:

* Forward match → reach last 'e'
* Backward shrink → find smallest window `"bcde"`
* Continue to next possible match → another candidate `"bdde"`
  Choose the shorter.

---

## ✅ Java Solution

```java
import java.util.*;

public class Main{
    public static String minWindow(String s, String p) {
        int m = s.length(), n = p.length();
        int minLen = Integer.MAX_VALUE;
        int minStart = -1;
        
        int i = 0, j = 0;
        while(i < m) {
            // forward scan
            if(s.charAt(i) == p.charAt(j)) {
                j++;
                // if fully matched
                if(j == n) {
                    int end = i;
                    j--;
                    // backward scan
                    int start = i;
                    while(j >= 0) {
                        if(s.charAt(start) == p.charAt(j)) {
                            j--;
                        }
                        start--;
                    }
                    start++;
                    
                    int winLen = end - start + 1;
                    if(minLen > winLen) {
                        minLen = winLen;
                        minStart = start;
                    }
                    
                    i = start;
                    j = 0;
                }
            }
            i++;
        }
        
        if(minStart == -1) return "";
        return s.substring(minStart, minStart + minLen);
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n · m)
  (Forward match + backward shrink when full match found)
* **Space Complexity:** O(1)
  (Only pointers)
