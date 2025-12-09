## String Compression

#### Statement

Given an array of characters, `chars`, compress it in place according to the following rules:

Start with an empty string `s`.

For each group of consecutive repeating characters in `chars`:

* If the group length is `1`, append just the character to `s`.
* Otherwise, append the character followed by the group length.

The compressed string `s` should not be returned separately; instead, it must be written directly into the input character array `chars`. Note that if a group’s length is `10` or greater, each digit of the length should be stored as a separate character in `chars`.

After modifying the array, return the new length of the compressed array.

**Note:** Your solution must use only constant extra space. Any characters beyond the returned length in `chars` can be ignored.

---

#### ✔️ Constraints

* `1 ≤ chars.length ≤ 2000`
* `chars[i]` is a lowercase English letter, an uppercase English letter, a digit, or a symbol.

---

## 🎯 Intuition

Use a two-pointer strategy:

| Pointer | Purpose                                   |
| ------- | ----------------------------------------- |
| `i`     | Scan through characters to count groups   |
| `write` | Write compressed output back into `chars` |

Steps:

1. Count consecutive identical characters
2. Write the character to `write`
3. If count > 1 → convert number to digits and write them
4. Continue until end

This ensures:

✔ In-place modification
✔ No extra memory
✔ Linear scan

---

## ✅ Java Solution

```java
public class Solution {
    public int compress(char[] chars) {
        int n = chars.length;
        int write = 0, i = 0;

        while (i < n) {
            char curr = chars[i];
            int count = 0;

            while (i < n && chars[i] == curr) {
                i++;
                count++;
            }

            chars[write++] = curr;

            if (count > 1) {
                for (char c : String.valueOf(count).toCharArray()) {
                    chars[write++] = c;
                }
            }
        }

        return write;
    }
}
```
#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
