## Substring with Concatenation of All Words

#### **Statement**

You are given a string, `s`, and an array of strings, `words`. All strings in `words` are of the same length.

A **concatenated string** is a string that contains all the words in `words` exactly once, in any order, concatenated together without any intervening characters.

Formally, a concatenated string is a permutation of all words joined together. For example, if `words = ["ab", "cd", "ef"]`, then the following are all valid concatenated strings: "abcdef", "abefcd", "cdabef", "cdefab", "efabcd", "efcdab". However, `"acdbef"` is not valid because it is not formed by concatenating all the words in any order.

Your task is to return all starting indices of substrings in `s` that are concatenated strings.

You may return the indices in any order.

---

#### ✔️ Constraints

* `1 ≤ s.length ≤ 10³`
* `1 ≤ words.length ≤ 1000`
* `1 ≤ words[i].length ≤ 30`
* All strings consist of lowercase English letters

---

## 🎯 Intuition

Let:

* `wLen` = length of each word
* `wCount` = number of words
* `block = wLen * wCount` = size of the full concatenation window

We want substrings of length `block` that contain **each word with the correct frequency**.

### Why sliding window works here

We treat the window in *chunks of word length*, not characters.

Example:
If each word is of length 3, then a valid window splits into:

```
[0..2], [3..5], [6..8], ...
```

#### Multi-phase sliding

Because words are fixed-length, substring boundaries must align with them.

So we start sliding from:

```
start = 0, 1, 2, ..., wLen - 1
```

For each phase:

1. Use a count map `seen` to track word frequencies inside the window.
2. Expand by `right += wLen` and read the next chunk.
3. If the chunk is invalid or over-frequent, shrink from `left`.
4. When the window contains all words with exact required counts:

   → record `left` as a valid starting index.

This ensures each index is processed only once per phase and avoids rebuilding counts repeatedly — giving a clean and efficient solution.

---

## ✅ Java Solution (Clean, Simple Sliding Window)

```java
import java.util.*;

public class Solution{
    public static List<Integer> findSubstring (String s, String[] words) {
        int n = words.length;
        int wordLen = words[0].length();
        int totalLen = n * wordLen;
        HashMap<String, Integer> map = new HashMap<>();
        
        for(String str : words) {
            map.put(str, map.getOrDefault(str, 0) + 1);
        }
        
        List<Integer> ans = new ArrayList<Integer>();
        
        for(int i = 0; i < wordLen; i++) {
            int l = i, r = i;
            HashMap<String, Integer> seen = new HashMap<>();
            
            // traverse by word chunks and match
            while(r + wordLen <= s.length()) {
                String word = s.substring(r, r + wordLen);
                r += wordLen;
                
                // if the word exists in the map, add to seen
                if(map.containsKey(word)) {
                    seen.put(word, seen.getOrDefault(word, 0) + 1);
                    
                    // now check if the freq is same or less
                    // if not, contract
                    while(seen.get(word) > map.get(word)) {
                        String left = s.substring(l, l + wordLen);
                        seen.put(left, seen.getOrDefault(left, 0) - 1);
                        l += wordLen;
                    }
                    
                    // after all contractions verify the window and record the left
                    if(r - l == totalLen) ans.add(l);
                } else {
                    seen.clear();
                    l = r;
                }
            }
        }
        return ans;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n × wLen) ≈ O(n)
  (Each index in each phase is processed once)
* **Space Complexity:** O(words.length) for frequency maps
