## Text Justification

#### **Statement**

You are given an array of strings, words, and an integer, maxWidth. Your task is to reformat the text by arranging the words into lines of a specified width. Each line must have exactly maxWidth characters.

Words should be packed into lines using a greedy approach, which means you should fit as many words as possible onto each line before moving to the next. For this purpose, if the length of the words is less than the maxWidth, then add empty spaces ‘ ’ to adjust the length of each line.

The formatting rules are as follows:

- For all lines except the last one, distribute extra spaces between words as evenly as possible to align both left and right.

- If the spaces don’t divide evenly, the leftmost gaps should receive the extra spaces ‘ ’.

- The last line of text and any line with only a single word must be left-justified. This means words are separated by a single space separates words, and the remaining width is filled with spaces on the right.

You can assume the following:

- A word is defined as a character sequence consisting of non-space characters only.

- Each word’s length is guaranteed to be greater than 0 and not exceed maxWidth.

- The input array words contains at least one word.

---

#### ✔️ Constraints

* `1 ≤ words.length ≤ 300`
* `1 ≤ words[i].length ≤ 20`
* `words[i]` consists of only English letters and symbols.
* `1 ≤ maxWidth ≤ 100`
* `words[i].length ≤ maxWidth`

---

## 🎯 Intuition

The problem requires constructing lines of length `maxWidth` using a **greedy packing strategy**:

* Fill as many words as possible into the current line.
* If it's not the last line:

  * Compute total spaces needed = `maxWidth - totalChars`
  * Evenly distribute spaces between words.
  * If there are leftover spaces, assign extra ones starting from the left.
* For the last line or lines with a single word:

  * Place one space between words.
  * Pad trailing spaces until line length becomes `maxWidth`.

Key observations:

* Greedy choice ensures each line is packed optimally.
* Space distribution logic differs for the last line.

---

## ✅ Java Solution

```java
import java.util.*;

class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> result = new ArrayList<>();
        int index = 0;

        while (index < words.length) {
            int totalChars = words[index].length();
            int last = index + 1;

            // Fit as many words as possible into the current line
            while (last < words.length) {
                if (totalChars + 1 + words[last].length() > maxWidth) break;
                totalChars += 1 + words[last].length();
                last++;
            }

            StringBuilder sb = new StringBuilder();
            int gaps = last - index - 1;

            // If last line or line with one word: left-justified
            if (last == words.length || gaps == 0) {
                for (int i = index; i < last; i++) {
                    sb.append(words[i]);
                    if (i < last - 1) sb.append(" ");
                }
                while (sb.length() < maxWidth) sb.append(" ");
            } else {
                // Middle justification
                int spaces = (maxWidth - totalChars) / gaps;
                int extra = (maxWidth - totalChars) % gaps;

                for (int i = index; i < last; i++) {
                    sb.append(words[i]);
                    if (i < last - 1) {
                        for (int s = 0; s < spaces + 1; s++) sb.append(" ");
                        if (extra-- > 0) sb.append(" ");
                    }
                }
            }

            result.add(sb.toString());
            index = last;
        }

        return result;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** `O(n * k)` — `n` words, `k` max line width (string building cost)
* **Space Complexity:** `O(n * k)` — output storage