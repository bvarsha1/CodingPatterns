## Reverse Words in a String

You are given a string `sentence` which may contain:

- Leading and trailing spaces
- Multiple spaces between words

**Goal:**  
Reverse the order of words while:

- Keeping characters inside each word unchanged
- Ensuring only a **single space** exists between words
- Removing **extra**, leading, and trailing spaces

---

### Constraints

- `1 ≤ sentence.length ≤ 10⁴`
- Sentence contains English letters, digits, and spaces
- At least one valid word is present

---

## Intuition

1. **Trim** the sentence to remove outer spaces
2. **Split** based on one or more spaces to extract valid words
3. **Reverse** the list of words using a two-pointer approach
4. **Join** them back with a single space

---

## Java Solution

```java
import java.util.*;

public class Solution {
    public static String reverseWords(String sentence) {
        sentence = sentence.trim();
        String[] words = sentence.split("\\s+");
        int left = 0, right = words.length - 1;
        while(left < right) {
            swapWords(words, left++, right--);
        }
        return String.join(" ", words);
    }
    
    public static void swapWords(String[] words, int i, int j) {
        String temp = words[i];
        words[i] = words[j];
        words[j] = temp;
    }
}
```
- Time Complexity: O(n)
- Space Complexity: O(n)