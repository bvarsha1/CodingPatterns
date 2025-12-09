## Minimum Number of Moves to Make Palindrome

### Statement
You are given a string `s`.  
In one move, you may **swap any two adjacent characters** in the string.

Return the **minimum number of adjacent swaps** required to transform `s` into a palindrome.

It is guaranteed that the string can be rearranged into a palindrome.

---

### Constraints
- `1 ≤ s.length ≤ 2000`
- `s` consists of lowercase English letters
- Guaranteed to be convertible into a palindrome

---

## Intuition

We want to form matching character pairs **from the outside toward the center**:

Use **two pointers**:
- `l` starts at the beginning
- `r` starts at the end

At each step:
1. If `s[l] == s[r]`  
   → Characters are already paired → move both inward.

2. Otherwise, search from `r` backward to find a matching character for `s[l]`:
   - If found at some index `k` (where `l < k ≤ r`)  
     → Bring `s[k]` to position `r` by swapping **adjacent characters**  
     → Count the number of swaps: `r - k`

3. If **no match exists** (only possible for the middle unique character):
   - Swap the unmatched character **toward the center** one step at a time
   - Adds 1 move per swap

This greedy approach ensures:
- We always match pairs with the fewest swaps possible
- Special handling for the **one odd-frequency character** (middle element)

---

## Java Solution (Greedy Two-Pointer)

```java
class Solution {
    public int minMovesToMakePalindrome(String s) {
        char[] arr = s.toCharArray();
        int moves = 0;
        int l = 0, r = arr.length - 1;
        
        while (l < r) {
            // 2 cases: 
            // i. We find matching characters, and move inwards
            // ii. We have to perform moves
            if (arr[l] == arr[r]) {
                l++;
                r--;
            } else {
                int k = r;
                // find the match on right side of l ptr.
                while (k > l && arr[k] != arr[l]) {
                    k--;
                }
                
                if (k == l) {
                    // Odd middle character case
                    swap(arr, l, l + 1);
                    // move inward to the middle
                    moves++;
                } else {
                    // Swap arr[k] to arr[r]
                    while (k < r) {
                        swap(arr, k, k + 1);
                        k++;
                        moves++;
                    }
                    // once processing is done, move inwards
                    l++;
                    r--;
                }
            }
        }
        return moves;
    }
    
    private void swap(char[] arr, int i, int j) {
        char temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```
- **Time Complexity: O(n^2)**
- **Space Complexity: O(n)**