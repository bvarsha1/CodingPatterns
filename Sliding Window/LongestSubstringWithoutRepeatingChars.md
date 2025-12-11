## **Longest Substring without Repeating Characters**

#### **Statement**

Given a string, str, return the length of the longest substring without repeating characters.

---

#### ✔️ **Constraints**
* `1 ≤ str.length ≤ 10^5`
* str consists of English letters, digits, and spaces.

---

## 🎯 **Intuition**

We want the **longest substring with all unique characters**.

A brute-force approach would check all substrings → **O(n²)**, which is too slow for length up to 10⁵.

Instead, we use the **sliding window + HashMap** technique:

#### 🔹 Key Idea

Maintain a window `[i … j]` such that:

* All characters in the window are **unique**
* `map` stores the **last index** where each character appeared

As we expand the window (`j` moves right):

* When we see a repeated character, we **shrink the window from the left** by moving `i` to `lastOccurrence + 1`.

This guarantees that the window always stays **duplicate-free**.

#### 🔹 Why `i = map.get(curr) + 1`?

If a character repeats and its last occurrence is inside the current window, the new valid window must start *after* the previous occurrence.

Example:
`abcda`
When second `a` appears, previous `a` is at index 0 → new window must start at index `1`.

#### 🔹 Window size = `j - i + 1`

We compute this at each iteration to track the maximum unique substring.

This method ensures:

* Each character is processed at most twice (enter + exit window)
* Overall runtime **O(n)**

---

## ✅ **Java Solution**

```java
import java.util.*;

public class Main {
  public static int findLongestSubstring(String str) {
    Map<Character, Integer> map = new HashMap<>(); // char -> last index
    int maxLen = 1;
    
    int i = 0;
    for (int j = 0; j < str.length(); j++) {
      char curr = str.charAt(j);
      
      if (map.containsKey(curr) && map.get(curr) >= i) {
        i = map.get(curr) + 1;   // shrink window
      }
      
      map.put(curr, j);          // update last index
      maxLen = Math.max(maxLen, j - i + 1);
    }
    
    return maxLen;
  }
}
```

#### ⏱️ **Complexity**

* **Time Complexity:** O(n)
  Each character index is added/updated once.

* **Space Complexity:** O(1)
  At most 256 characters if the set is ASCII (still constant), or O(min(n, charset)).
