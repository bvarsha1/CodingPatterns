## Largest Palindromic Number

#### **Statement**

You are given a string `num` consisting of digits from `0` to `9`. Your task is to return the largest possible palindromic number as a string by using some or all of the digits in `num`. The resulting palindromic number must not have leading zeros.

> **Note:** You may reorder the digits freely, and you must use at least one digit from the `num` string.

---

#### ✔️ Constraints

* 1 ≤ `num.length` ≤ 1000
* `num` consists of digits

---

## 🎯 Intuition

To form the **largest palindromic number**, we use a **greedy strategy**:

* A palindrome is defined by its **left half**, optional **center digit**, and a mirrored **right half**
* To maximize the value:

  * Place the **largest digits** on the outside
  * Use digit frequencies to form **pairs** for the left and right halves
  * If any digit remains unused, place the **largest such digit in the center**
* Leading zeros are not allowed, so `0` cannot start the palindrome unless it is the **only possible digit**

This greedy construction works because choosing larger digits earlier always leads to a lexicographically larger palindrome.

---

## ✅ Java Solution (Refined from your approach)

```java
public class Solution {
    public static String largestPalindrome(String num) {
        int[] freq = new int[10];

        for (char ch : num.toCharArray()) {
            freq[ch - '0']++;
        }

        StringBuilder left = new StringBuilder();

        // Build the left half using largest digits
        for (int d = 9; d >= 0; d--) {
            // Avoid leading zero
            if (d == 0 && left.length() == 0) continue;

            int pairs = freq[d] / 2;
            for (int i = 0; i < pairs; i++) {
                left.append((char) (d + '0'));
            }
            freq[d] -= pairs * 2;
        }

        // Pick the largest remaining digit as center (if any)
        char center = '\0';
        for (int d = 9; d >= 0; d--) {
            if (freq[d] > 0) {
                center = (char) (d + '0');
                break;
            }
        }

        // Edge case: only zeros exist
        if (left.length() == 0 && center == '\0') {
            return "0";
        }

        StringBuilder right = new StringBuilder(left).reverse();

        if (center != '\0') {
            left.append(center);
        }

        return left.append(right).toString();
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1) — fixed-size frequency array
