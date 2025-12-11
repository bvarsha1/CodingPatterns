## Repeated DNA Sequences

#### **Statement**

A DNA sequence consists of nucleotides represented by the letters ‘A’, ‘C’, ‘G’, and ‘T’ only. For example, “ACGAATTCCG” is a valid DNA sequence.

Given a string, s, that represents a DNA sequence, return all the 10-letter-long sequences (continuous substrings of exactly 10 characters) that appear more than once in s. You can return the output in any order.

---

#### ✔️ Constraints

1 ≤ s.length ≤ 10^3
s[i] is either 'A', 'C', 'G', or 'T'.

---

## 🎯 Intuition

We must find **10-character substrings** that occur **more than once**.

Efficient approach:

* Slide a window of size **10**
* Use a **HashSet** to track sequences seen once
* Use another **HashSet** (or result list) to track sequences seen again → **duplicates**

We only record sequences that **appear multiple times**.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static List<String> findRepeatedDnaSequences(String s) {
        Set<String> seen = new HashSet<>();
        Set<String> repeated = new HashSet<>();

        for (int i = 0; i + 10 <= s.length(); i++) {
            String sub = s.substring(i, i + 10);
            if (!seen.add(sub)) {
                repeated.add(sub);
            }
        }

        return new ArrayList<>(repeated);
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)
