## Largest Odd Number in String

#### **Statement**

You are given a string, `num`, which represents a large integer. Your task is to find the largest odd-valued integer that can be formed as a non-empty substring of `num`. Return this odd integer as a string. If no odd integer exists, return an empty string (`""`).

Note: A substring is a continuous sequence of characters within a string.

---

#### ✔️ Constraints

* 1 ≤ `num.length` ≤ 10^4
* `num` only consists of digits and does not contain any leading zeros

---

## 🎯 Intuition

To form the largest odd-valued substring:

* Any substring that forms an odd integer must end at an odd digit
* Therefore, we scan from **right to left**
* The **first odd digit found** at index `i` guarantees `num[0...i]` is the largest odd substring
* If no odd digit exists, return an empty string

This greedy check is optimal because removing suffix characters preserves a larger numeric value.

---

## ✅ Java Solution

```java
class Solution {
    public String largestOddNumber(String num) {
        for (int i = num.length() - 1; i >= 0; i--) {
            int digit = num.charAt(i) - '0';
            if ((digit & 1) == 1) { 
                return num.substring(0, i + 1);
            }
        }
        return "";
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)