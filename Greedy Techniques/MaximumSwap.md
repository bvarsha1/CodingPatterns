## Maximum Swap

#### **Statement**

Given an integer `num`, return the maximum number that can be formed by swapping at most two digits once.

---

#### ✔️ Constraints

* 0 ≤ `num` ≤ 10<sup>5</sup>

---

## 🎯 Intuition

To maximize the number, we want the **largest digit as left as possible**. If a larger digit exists to the right of a position, swapping them increases the number.

Approach:

1. Convert to char array
2. Scan from right to left, tracking the index of the maximum digit seen so far
3. When you find a digit smaller than this max, record the swap indices
4. After the scan, perform the best possible swap (if any)

This guarantees the optimal single-swap result.

---

## ✅ Java Solution

```java
class Solution {
    public int maximumSwap(int num) {
      String n = String.valueOf(num);
      char[] digits = n.toCharArray();
      
      int max = n.length() - 1;
      int curr = -1, lastMax = -1;
      
      for(int i = n.length() - 1; i >= 0; i--) {
        if(digits[i] > digits[max]) {
          max = i;
        } else if(digits[i] < digits[max]) {
          curr = i;
          lastMax = max;
        }
      }
      
      if(curr != -1 && lastMax != -1) {
        char c = digits[curr];
        digits[curr] = digits[lastMax];
        digits[lastMax] = c;
      }
      
      return Integer.parseInt(new String(digits));
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)