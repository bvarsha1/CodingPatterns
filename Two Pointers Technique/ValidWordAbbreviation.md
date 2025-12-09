## Valid Word Abbreviation

### Statement
Given a string `word` and an abbreviation `abbr`, return **TRUE** if the abbreviation is valid for the given word, otherwise return **FALSE**.

An abbreviation can replace **non-adjacent**, **non-empty** substrings of the word with their **lengths**.

Rules:
- Numbers in abbreviation represent the count of skipped characters.
- Leading zeros are **not allowed**.
- Skip lengths must be **> 0**.
- Replaced substrings must **not be adjacent**; meaning abbreviation should match characters in order.

---

### Examples of Valid Abbreviations
| Word | Abbreviation | Reason |
|------|--------------|--------|
| `calendar` | `cal3ar` | Skips `"end"` (length 3) |
| `calendar` | `c6r` | Skips `"alenda"` (length 6) |
| `internationalization` | `i18n` | Skips 18 characters |

---

### Examples of Invalid Abbreviations
| Abbreviation | Reason |
|-------------|--------|
| `c06r` | Leading zero not allowed |
| `cale0ndar` | Zero skip not allowed (empty skip) |
| `c24r` | Skipped substrings become adjacent |

---

### Constraints
- `1 ≤ word.length ≤ 20`
- word contains only lowercase English letters
- `1 ≤ abbr.length ≤ 10`
- abbr contains lowercase letters and digits
- All numbers in abbr fit in 32-bit integer

---

## Intuition

We use **two pointers**:
- `i` → traverses the original word
- `j` → traverses the abbreviation

While scanning `abbr`:
- If `abbr[j]` is a letter → it must match `word[i]`; move both
- If it's a digit → parse the full number; ensure:
  - No leading zero
  - Number > 0  
  Then **jump ahead** in `word` by that number

Finally:
- Both pointers must reach the end **at the same time** for a valid match

---

## Java Solution

```java
public class Solution{
    public static boolean validWordAbbreviation(String word, String abbr) {
        int i = 0, j = 0;
        while(j < abbr.length()) {
          if(Character.isDigit(abbr.charAt(j))) {
            if(abbr.charAt(j) == '0') return false;
            int n = 0;
            while(j < abbr.length() && Character.isDigit(abbr.charAt(j)) ) {
              n = (n * 10) + (abbr.charAt(j++) - '0');
            }
            i += n;
          } else {
            if(i >= word.length() || word.charAt(i) != abbr.charAt(j)) {
              // i should not go out of index
              // char at i == char at j
              return false;
            }
            i++;
            j++;
          }
        }
        
        // make sure both indexes are at the end of respective strings
        return i == word.length() && j == abbr.length();
    }
}
```
- **Time Complexity: O(m)**
  - We iterate through the abbreviation exactly once.
  - Pointer jumps in the original word do not add processing time per character.
- **Space Complexity: O(1)**
  - Uses constant additional memory.