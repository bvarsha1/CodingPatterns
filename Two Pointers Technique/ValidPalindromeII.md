## Valid Palindrome II

#### Statement

Write a function that takes a string as input and checks whether it can be a valid palindrome by removing **at most one character** from it.

---

#### ✔️ Constraints

* `1 ≤ string.length ≤ 10⁵`
* The string consists **only of English letters**

---

## 🎯 Intuition

Use a **two-pointer** approach:

* Compare characters from both ends
* If mismatch occurs → we get **one chance** to skip a character
* Check either:

  * Skip left pointer (`l + 1`)
  * Skip right pointer (`r - 1`)
* If either path forms a palindrome → return `true`

This ensures:

* We try at most **one deletion**
* No extra space required

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static boolean validPalindrome(String s) {
        int l = 0, r = s.length() - 1;

        while (l < r) {
            if (s.charAt(l) != s.charAt(r)) {
                return isPalindrome(s, l + 1, r) || isPalindrome(s, l, r - 1);
            }
            l++;
            r--;
        }

        return true;
    }

    private static boolean isPalindrome(String s, int l, int r) {
        while (l < r) {
            if (s.charAt(l) != s.charAt(r)) return false;
            l++;
            r--;
        }
        return true;
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)