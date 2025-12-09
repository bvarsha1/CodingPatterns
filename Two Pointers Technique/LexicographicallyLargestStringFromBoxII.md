## Find the Lexicographically Largest String From Box II

#### Statement

You are given a string, `word`, and an integer `numFriends`, representing the number of friends participating in a game organized by Alice.

The game consists of multiple rounds, and in each round:

* The string `word` is split into exactly `numFriends` **non-empty** substrings.
* The split must be **unique**; no previous round has produced the same sequence of splits.
* All resulting substrings from the split are placed into a box.

When all rounds are over and all possible unique splits have been performed, determine the **lexicographically largest string** among all the substrings in the box.

A string **a** is considered lexicographically larger than a string **b** if:

* At the first position where they differ, the character in **a** appears later in the alphabet than in **b**, OR
* If **a** is a prefix of **b**, then **b**, being longer, is considered larger.

---

#### ✔️ Constraints

* `1 ≤ word.length ≤ 10³`
* `word` consists only of lowercase English letters
* `1 ≤ numFriends ≤ word.length`

---

## 🎯 Intuition

Every valid split produces `numFriends` non-empty parts, and **all substrings that could appear** from any such split are simply **all substrings of `word` having length ≥ (word.length − numFriends + 1)**.

👉 We want the **lexicographically largest** substring of possible maximum length:

> Required substring length = `word.length − numFriends + 1`

So we need to find the *best starting index* of such a substring.

We scan with **two pointers (i, j)**:

| Pointer | Meaning                      |
| ------- | ---------------------------- |
| `i`     | current best candidate start |
| `j`     | challenger start index       |

At each step, compare substrings starting at `i` and `j`.
When `word[j..]` becomes lexicographically better → move `i` to `j`.

We cleverly skip already-compared characters → ensuring **O(n)** time.

---

## ✅ Java Solution

```java
public class Solution
{
    public String answerString(String word, int numFriends) 
    {
        if(numFriends == 1) return word;
        
        int i = 0, j = 1;
        int len = word.length();
        while(j < len) {
            int k = 0;
            while(j + k < len && word.charAt(i + k) == word.charAt(j + k)) k++;
            
            if(j + k < len && word.charAt(i + k) < word.charAt(j + k)) {
                int tempIdx = i;
                i = j;
                j = Math.max(j + 1, tempIdx + k + 1);
            } else {
                j = j + k + 1;
            }
        }
        return word.substring(i, Math.min(len, i + len - numFriends + 1));
    }
}
```
* **Time Complexity:** O(n) — efficient two-pointer skipping strategy
* **Space Complexity:** O(1) — only pointer variables used
