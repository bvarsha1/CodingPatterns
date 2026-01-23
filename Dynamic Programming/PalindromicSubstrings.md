## Palindromic Substrings

#### **Statement**

Given a string, `s`, return the number of palindromic substrings contained in it. A substring is a contiguous sequence of characters in a string. A palindrome is a phrase, word, or sequence that reads the same forward and backward.

---

#### ✔️ Constraints

* 1 ≤ `s.length` ≤ 1000
* `s` consists of only lowercase English characters.

---

## 🎯 Intuition

We need to count how many substrings are palindromes.
Brutally checking every substring produces O(n³) time (generate + check).

A better approach uses **expand around center**:

* Every palindrome has a “center”
* For length `n`, there are:

  * `n` odd centers (each character)
  * `n-1` even centers (between characters)

For each center, expand while characters match on both sides. Each successful expansion counts as a palindromic substring.

---

## 🧩 Approach

1. Initialize a counter `count = 0`
2. Loop `i` from `0` to `n-1`
3. Expand from:

   * `(i, i)` → odd-length palindromes
   * `(i, i+1)` → even-length palindromes
4. For each expansion, if characters match → increment count and expand further
5. Return `count`

---

## ✅ Java Solution

```java
public class Main{
  public static int countPalindromicSubstrings(String s) {
    int count = 0;
    
    for(int i = 0; i < s.length(); i++) {
      count += countPalindromesAroundCentre(s, i, i);
      count += countPalindromesAroundCentre(s, i, i + 1);
    }
    
    return count;
  }
  
  public static int countPalindromesAroundCentre(String s, int i, int j) {
    int count = 0;
    while(i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j)) {
      i--;
      j++;
      count++;
    }
    
    return count;
  }
}
```

## ⏱️ Complexity

* **Time Complexity:** `O(n²)` — expanding around each center
* **Space Complexity:** `O(1)` — no extra space besides counters