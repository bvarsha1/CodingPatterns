## Word Break

#### **Statement**

Given a string, `s`, and a dictionary of strings, `wordDict`, check if `s` can be segmented into a space-separated sequence of one or more dictionary words. If yes, return TRUE; else, return FALSE.

> **Note:** The same word in the dictionary may be used multiple times.

---

#### ✔️ Constraints

* 1 ≤ `s.length` ≤ 250
* 1 ≤ `wordDict.length` ≤ 1000
* 1 ≤ `wordDict[i].length` ≤ 20
* `s` and `wordDict[i]` consist of only lowercase English letters.
* All the strings of `wordDict` are unique.

---

## 🎯 Intuition

This problem asks if we can break string `s` into valid words from `wordDict`.
A direct greedy approach doesn’t work because some prefixes may fail temporarily but succeed later.

Best approach: **Dynamic Programming**

Idea:

* Let `dp[i] = true` if substring `s[0..i)` can be segmented.
* `dp[0] = true` (empty string is valid)
* For each `i`, check all `j < i`:

  * If `dp[j] == true` and `s[j..i)` is in dictionary → mark `dp[i] = true`

We use a `HashSet` for fast lookups.

---

## 🧩 Approach

1. Insert all `wordDict` words into a `HashSet`
2. Initialize DP array of size `n+1` with `dp[0] = true`
3. For each index `i` from `1..n`:

   * For each `j` from `0..i-1`:

     * If `dp[j] == true` AND `s[j..i)` exists in dict → `dp[i] = true`
4. Return `dp[n]`

---

## ✅ Java Solution

```java
import java.util.*;

public class Main{
  public static boolean wordBreak (String s, List<String> wordDict ) {
    HashSet<String> words = new HashSet<>(wordDict);
    int n = s.length();
    boolean[] dp = new boolean[n + 1];
    dp[0] = true;
    
    // iterate each char of the string
    for(int i = 1; i <= n; i++) {
      // iterate from 0 to i
      for(int j = 0; j < i; j++) {
        if(dp[j] && words.contains(s.substring(j, i))) {
          dp[i] = true;
          // break as soon as even 1 such substr found
          break;
        }
      }
    }
    
    return dp[n];
  }
}
```

## ⏱️ Complexity

* **Time Complexity:** `O(n²)`
  DP has nested loops (`i` and `j`) and substring checks
* **Space Complexity:** `O(n)`
  DP array + dictionary set