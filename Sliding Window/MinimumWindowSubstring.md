## **Minimum Window Substring**

#### **Statement**

Given two strings, **s** and **t**, find the minimum window substring in **s**, which has the following properties:

* It is the shortest substring of s that includes all of the characters present in t.
* It must contain at least the same frequency of each character as in t.
* The order of the characters does not matter here.

**Note:**
If there are multiple valid minimum window substrings, return any one of them.

---

#### ✔️ **Constraints**

- Strings **s** and **t** consist of uppercase and lowercase English characters.

- `1 ≤ s.length, t.length ≤ 10³`

---

## 🎯 **Intuition**

This is a **classic sliding window with frequency maps** problem.

We're trying to find the **smallest substring of s** that contains **all characters of t with their required frequencies**.

To achieve this efficiently:

#### 🔹 1. Count frequencies required by `t`

We store all needed characters and their counts in a **target map (`tmap`)**.

Example:
t = `"AABC"`
→ A:2, B:1, C:1

#### 🔹 2. Expand the window over `s`

We move a right pointer `i` and build a **window map (`smap`)**.

Each time we add a character, if it helps meet the requirement (i.e., smap[ch] ≤ tmap[ch]),
we increment **count**, meaning "one more useful character matched".

When `count == t.length()` → **we have a valid window**.

#### 🔹 3. Contract the window (shrink from the left)

Once we have a valid window, we try to shrink it:

We keep removing characters from the left (`start`) **as long as:**

* either it's not a required character, OR
* we have extra copies of that character in the window

This ensures we keep the window valid but as small as possible.

#### 🔹 4. Record the best (minimum) window

Each time after shrinking:

* compute `winLen = i - start + 1`
* update `minLen` and `minStart` if this window is smaller

#### ✔️ Why this works

Because:

* Every character enters and exits the window at most once → O(n)
* We only shrink when the window is valid
* We ensure we keep only minimum-length valid windows

This leads to an **optimal O(n)** solution—far faster than checking all substrings.

---

## ✅ **Java Solution**

```java
import java.util.*;

public class Main {
    public static String minWindow(String s, String t) {
        HashMap<Character, Integer> tmap = new HashMap<>();
        HashMap<Character, Integer> smap = new HashMap<>();
        
        for(char ch : t.toCharArray()) {
            tmap.put(ch, tmap.getOrDefault(ch , 0) + 1);
        }
        
        int count = 0;
        int minStart = -1, start = 0;
        int minLen = Integer.MAX_VALUE;
        
        int i = 0;
        while (i < s.length()) {
            char ch = s.charAt(i);
            smap.put(ch, smap.getOrDefault(ch, 0) + 1);
            
            if (tmap.containsKey(ch) && tmap.get(ch) >= smap.get(ch))
                count++;
            
            // full match achieved
            if (count == t.length()) {

                // try shrinking from left
                while (!tmap.containsKey(s.charAt(start)) ||
                       tmap.get(s.charAt(start)) < smap.get(s.charAt(start))) {
                       
                    smap.put(s.charAt(start), smap.get(s.charAt(start)) - 1);
                    start++;
                }
                
                int winLen = i - start + 1;
                if (minLen > winLen) {
                    minLen = winLen;
                    minStart = start;
                }
            }
            i++;
        }
        
        if (minStart == -1) return "";
        return s.substring(minStart, minStart + minLen);
    }
}
```

#### ⏱️ **Complexity**
- **Time Complexity:** O(n)
- **Space Complexity:** O(1)
    - at most 52 characters (upper/lowercase) stored in maps
