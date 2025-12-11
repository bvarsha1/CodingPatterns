## **Longest Repeating Character Replacement**

#### **Statement**

Given a string, s, and an integer, k, find the length of the longest substring in s, where all characters are identical, after replacing, at most, k characters with any other uppercase English character.

---

#### ✔️ **Constraints**

1 ≤ s.length ≤ 10^3
s consists of only uppercase English characters.
0 ≤ k ≤ s.length

---

## 🎯 **Intuition**

We want the **longest substring that can be turned into all identical characters** by replacing **at most k characters**.

#### 🔹 Key Insight

If you look at any window (substring):

* Let `freq[max]` = count of the **most frequent character** in that window.
* Window length = `right - left + 1`
* Characters to replace = `(window length - freq[max])`

To make the whole window uniform, we must replace all characters *except* the most frequent one.

So a window is **valid** if:

```
(window length - maxFreq) ≤ k
```

This means:
We can afford replacing the "non-maxFreq" characters using at most k allowed replacements.

#### 🔹 Sliding Window Strategy

1. Expand the window using `right` pointer.
2. Track frequencies of characters.
3. Track `maxFreq` seen so far.
4. If window becomes invalid (needs more than k replacements), shrink from left.
5. Keep track of the **maximum valid window length**.

#### 🔹 Why does this work?

Because:

* We never reduce `maxFreq` when shrinking the window — it's allowed.
* Even if `maxFreq` is slightly outdated, the window validity check still works.
* This ensures a clean **O(n)** solution without recalculating frequencies each time.

---

## ✅ **Java Solution**

```java
class LongestRepeatingCharacterReplacement {

    public static int characterReplacement(String s, int k) {
        int[] freq = new int[26];
        int left = 0, maxFreq = 0, result = 0;

        for (int right = 0; right < s.length(); right++) {
            int idx = s.charAt(right) - 'A';
            freq[idx]++;
            maxFreq = Math.max(maxFreq, freq[idx]);

            int windowSize = right - left + 1;

            // If replacements needed exceed k, shrink window
            if (windowSize - maxFreq > k) {
                freq[s.charAt(left) - 'A']--;
                left++;
            }

            result = Math.max(result, right - left + 1);
        }

        return result;
    }
}
```

#### ⏱️ **Complexity**

* **Time Complexity:** O(n)
  Each character enters and leaves the window once.

* **Space Complexity:** O(1)
  Only a 26-sized frequency array.
