## Reverse String

#### Statement

You are given a character array, `s`, representing a string. Write a function that reverses the array **in-place**.

The reversal must be done by modifying the original array directly.
You **cannot** use extra memory beyond **constant** space `O(1)`.

---

#### ✔️ Constraints

* `1 ≤ s.length ≤ 1000`
* Each `s[i]` is a printable ASCII character.

---

## 🎯 Intuition

A straightforward two-pointer technique:

| Pointer | Starts at  | Action     |
| ------- | ---------- | ---------- |
| `left`  | 0          | Move right |
| `right` | last index | Move left  |

Swap characters at these pointers until they meet in the middle.

This guarantees:

* In-place modification ✔️
* Only constant extra space ✔️
* Linear traversal ✔️

---

## ✅ Java Solution

```java
public class Solution {
    public void reverseString(char[] s) {
        int left = 0, right = s.length - 1;
        while(left < right) {
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            left++;
            right--;
        }
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
