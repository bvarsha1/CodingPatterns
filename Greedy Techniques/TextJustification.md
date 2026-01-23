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

public class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        int i = 0;
        List<String> lines = new ArrayList<String>();
        
        while(i < words.length) {
            int lineStart = i;
            int lineLen = words[i].length();
            i++;
            
            while(i < words.length && lineLen + 1 + words[i].length() <= maxWidth) {
                lineLen += 1 + words[i].length();
                i++;
            }
            
            int lineEnd = i;
            int wordCount = lineEnd - lineStart;
            
            StringBuilder currLine = new StringBuilder();

            // left justified case
            if(lineEnd == words.length || wordCount == 1) {
                for(int k = lineStart; k < lineEnd; k++) {
                    currLine.append(words[k]);
                    if(k < lineEnd - 1) currLine.append(" ");
                }
                if(maxWidth - lineLen > 0) {
                    currLine.append(" ".repeat(maxWidth - lineLen));
                }
            }
            // equally justified case
            else {
                int remSpaces = maxWidth - lineLen;
                int gaps = wordCount - 1;
                int spaceSplit = remSpaces / gaps;
                int extraSpaces = remSpaces % gaps;
                
                for(int k = lineStart; k < lineEnd; k++) {
                    currLine.append(words[k]);
                    if(k < lineEnd - 1) {
                        int gapSpaces = spaceSplit + 1 + (extraSpaces > 0 ? 1 : 0);
                        System.out.println(gapSpaces);
                        currLine.append(" ".repeat(gapSpaces));
                        extraSpaces--;
                    }
                }
            }
            
            // add line to result set
            lines.add(currLine.toString());
        }
        
        return lines;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** `O(n * k)` — `n` words, `k` max line width (string building cost)
* **Space Complexity:** `O(n * k)` — output storage