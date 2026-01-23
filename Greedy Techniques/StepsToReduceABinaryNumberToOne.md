## Number of Steps to Reduce a Binary Number to One

#### **Statement**

You are given a string, `str`, as a binary representation of an integer. Your task is to return the number of steps needed to reduce it to `1` by following these rules:

* If the number is even, divide it by `2`.
* If the number is odd, add `1` to it.

You can always reach `1` for all provided test cases.

---

#### ✔️ Constraints

* 1 ≤ `str.length` ≤ 500
* `str` consists of characters `'0'` or `'1'`
* `str[0] == '1'`

---

## 🎯 Intuition

Simulating the number directly would be inefficient because the binary number can be very large. Instead, we operate **directly on the binary string**.

**Key observations:**

* Dividing by `2` in binary is equivalent to **removing the last bit**
* If the number is **odd** (ends with `'1'` and not `"1"` itself), adding `1` causes:
  * Trailing `'1'`s to flip to `'0'`
  * The first `'0'` before them to flip to `'1'`
* This behavior can be simulated using a **carry** while traversing from right to left

**Strategy:**

* Traverse the binary string from the least significant bit to the most significant
* Maintain a `carry` to represent whether we previously added `1`
* Count operations based on whether the current effective bit is `0` or `1`

This avoids expensive big-integer arithmetic.

---

## ✅ Java Solution

```java
public class Solution {
    public static int numSteps(String str) {
        int steps = 0;
        int carry = 0;

        // Traverse from right to left, stopping before the most significant bit
        for (int i = str.length() - 1; i > 0; i--) {
            int bit = (str.charAt(i) - '0') + carry;

            if (bit % 2 == 0) {
                // even -> divide by 2
                steps += 1;
            } else {
                // odd -> add 1, then divide by 2
                steps += 2;
                carry = 1;
            }
        }

        // If there's a carry left at the most significant bit
        if (carry == 1) {
            steps += 1;
        }

        return steps;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)