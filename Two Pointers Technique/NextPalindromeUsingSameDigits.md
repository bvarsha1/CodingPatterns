## Next Palindrome Using Same Digits

You are given a numeric string `numStr` that is already a palindrome.  
Return the *smallest palindrome strictly larger* than `numStr` using the **same digits**.  
If no such palindrome exists, return an empty string `""`.

---

#### Constraints
- `1 ≤ numStr.length ≤ 10^5`
- `numStr` is a palindrome
- Only digit characters `0–9`

---

## Intuition

A palindrome is determined entirely by its **left half**:

| Part | Role |
|------|-----|
| Left half | Controls lexicographic order |
| Middle digit (if odd length) | Remains unchanged |
| Right half | Mirror of left half |

So to form the **next** greater palindrome:
1. Extract the **left half** of the palindrome.
2. Compute the **next lexicographical permutation** of that half.
3. Mirror it to form a full palindrome.
4. If no next permutation exists → no larger palindrome can be formed → return `""`.

This avoids generating all permutations — optimal for long strings.

---

## Java Solution

```java
import java.util.*;

public class Solution {
    public static String findNextPalindrome(String numStr) {
        int n = numStr.length();
        char[] leftDigits = new char[n / 2];
        for(int i = 0; i < n / 2; i++) {
            leftDigits[i] = numStr.charAt(i);
        }
        if(!nextBigger(leftDigits)) return "";
        
        StringBuilder sb = new StringBuilder();
        String left = new String(leftDigits);
        sb.append(left);
        if(n%2 != 0) sb.append(numStr.charAt(n/2));
        sb.append(new StringBuilder(left).reverse());
        
        return sb.toString();
    }
    
    public static boolean nextBigger(char[] arr) {
        // find first strictly decreasing element
        int i = arr.length - 2;
        while(i >= 0 && arr[i] >= arr[i+1]) i--;
        
        if(i < 0) return false;
        
        // find the next bigger element to first decreasing element
        int j = arr.length - 1;
        while(arr[j] <= arr[i]) j--;
        
        // swap the 2
        char tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
        
        // reverse the rest, because its an increasing sequence from right
        reverse(arr, i + 1, arr.length - 1);
        return true;
    }
    
    public static void reverse(char[] arr, int i, int j) {
        while(i < j) {
            char tmp = arr[i];
            arr[i] = arr[j];
            arr[j] = tmp;
            i++;
            j--;
        }
    }
}
```
- **Time Complexity: O(n)**
- **Space Complexity: O(n)**