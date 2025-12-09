## Strobogrammatic Number

### Statement
Given a string `num` representing an integer, determine whether it is a strobogrammatic number. Return TRUE if the number is strobogrammatic or FALSE if it is not.

> **Note:** A **strobogrammatic number** appears the same when rotated 180180 degrees (viewed upside down). For example, “69” is strobogrammatic because it looks the same when flipped upside down, while “962” is not.

**Constraints:**

- 1<=1<= `num.length` <=50<=50
- `num` contains only digits.
- `num` has no leading zeros except when the number itself is zero.

---
## Intuition

A number is **strobogrammatic** if it looks the same when rotated 180 degrees.  
This means each digit must either:

1. **Stay the same** after rotation  
   - Valid: `0 → 0`, `1 → 1`, `8 → 8`

2. **Form a valid mirrored pair**  
   - Valid: `6 ↔ 9`, `9 ↔ 6`

Any digit that **cannot** be mirrored (e.g., `2, 3, 4, 5, 7`) makes the number invalid.

We use two pointers:
- `left` → start of the string  
- `right` → end of the string  

At each step:
- Check if `num[left]` and `num[right]` form a valid strobogrammatic pair
- Move inward until pointers cross

If all mirrored pairs are valid, the number remains the same when rotated → it is strobogrammatic.

---
## Java Solution
```java
public class Solution{
    public static boolean isStrobogrammatic (String num) 
    {
        // Replace this placeholder return statement with your code
        char[] map = new char[10];
        map[0] = '0';
        map[1] = '1';
        map[6] = '9';
        map[8] = '8';
        map[9] = '6';
        
        int left = 0, right = num.length() - 1;
        while(left <= right) {
            if(map[num.charAt(left) - '0'] != num.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```
- **Time Complexity: O(n)**
- **Space Complexity: O(1)**
  - Constant space of 10 digits