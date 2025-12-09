## Partition Labels

#### Statement

You are given a string s. Your task is to divide the string into as many parts as possible such that each letter appears in at most one part.

In other words, no character should occur in more than one partition. After concatenating all parts in order, the result should be the original string s.

For example, given s = "bcbcdd", a valid partition is ["bcbc", "dd"]. However, partitions like ["bcb", "cdd"] or ["bc", "bc", "dd"] are invalid because some letters appear in multiple parts.

Return a list of integers representing the sizes of these partitions.

---

#### ✔️ Constraints

* `1 ≤ s.length ≤ 500`
* `s` consists of lowercase English letters.

---

## 🎯 Intuition

We want to ensure **each character appears in only one partition**.

Key idea:

1. First, record the **last index** of every character in the string.
2. Then, iterate through the string and keep extending the current partition’s end to cover the **furthest last occurrence** of any character included so far.
3. Once the current index reaches that end → a partition is formed.

This works because:

✔ Every character in that range will not appear again
✔ We cut exactly where all involved characters finish
✔ Guarantees maximum number of valid partitions

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] last = new int[26];
        
        // 1️⃣ Store last occurrence of each character
        for (int i = 0; i < s.length(); i++) {
            last[s.charAt(i) - 'a'] = i;
        }

        List<Integer> result = new ArrayList<>();
        int start = 0, end = 0;

        // 2️⃣ Traverse and split optimally
        for (int i = 0; i < s.length(); i++) {
            // max bound end, till i == end
            end = Math.max(end, last[s.charAt(i) - 'a']);
            if (i == end) {
                result.add(end - start + 1);
                // start of new sequence
                start = i + 1;
            }
        }

        return result;
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
  * (array of fixed size 26)
