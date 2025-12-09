## Valid Palindrome

Given a string `s`, return `TRUE` if it is a palindrome; otherwise, return `FALSE`.

A phrase is considered a palindrome if it reads the same backward as forward **after**:
- Converting all uppercase letters to lowercase
- Removing all non-alphanumeric characters (only letters and digits are considered)

### Constraints
- `1 ≤ s.length ≤ 3000`
- `s` consists only of printable ASCII characters


---

## Intuition

To check if a given string is a palindrome, we must ensure that the sequence of characters looks the same from the front and the back.  
However, the string might contain:
- Spaces
- Punctuation
- Different letter cases

These should not affect the result.  
So, we must compare only normalized alphanumeric characters.

---

## How We Reach the Approach

1. **Normalize the Input**
   - Convert the string to lowercase so case doesn’t matter.
   - Remove all characters that are not letters or numbers.
   - Now we have a clean string to test.

2. **Check Symmetry**
   - Use two pointers: one from the start and one from the end.
   - Move each pointer inward and compare characters.
   - If all matching characters are equal → it’s a palindrome.

3. **Decision**
   - If any mismatch occurs → return `FALSE`
   - If all checks pass → return `TRUE`

This ensures an efficient solution that scans the string only once for filtering and once for validation — optimal for the constraint `s.length ≤ 3000`.

---
## Solution
#### Modifying the string
```java
class Solution {
    public boolean isPalindrome(String s) {
        s = s.replaceAll("[^a-zA-Z0-9]", "").toLowerCase();
        int left = 0, right = s.length() - 1;

        while (left < right) {
            if (s.charAt(left) != s.charAt(right))
                return false;
            left++;
            right--;
        }
        return true;
    }
}
```
- Time Complexity: O(n)
- Space Complexity: O(n)

#### Without modifying the string
```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;

        while (left < right) {
            while(left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            while(left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```
- Time Complexity: O(n)
- Space Complexity: O(1)
