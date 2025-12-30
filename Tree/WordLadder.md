## Word Ladder

#### Statement

Given two words, src and dest, and a list, words, return the number of words in the shortest transformation sequence from src to dest. If no such sequence can be formed, return 0.

A ***transformation sequence*** is a sequence of words
(src → word₁ → word₂ → word₃ → … → wordⱼ) that has the following properties:

* wordⱼ = dest
* Every pair of consecutive words differs by a single character
* All the words in the sequence are present in the words list
* The src does not need to be present in words

---

#### ✔️ Constraints

* `1 ≤ src.length ≤ 10`
* `src.length == dest.length == words[i].length`
* `src != dest`
* `1 ≤ words.length ≤ 5000`
* No duplicates in `words`
* `src`, `dest`, and `words[i]` consist of lowercase English characters

---

## 🎯 Intuition

This is a **shortest path problem** where:

* Each word is a **node**
* An edge exists between two words if they differ by **exactly one character**
* We need the **shortest transformation** from `src` to `dest`

👉 This immediately suggests **Breadth First Search (BFS)**, because:

* BFS finds the **shortest path** in an unweighted graph
* Each level of BFS corresponds to **one transformation step**

### Key observations

* `src` may not be in `words`, but it can still be the BFS start
* Once a word is used, it should not be reused → avoid cycles
* For each word, try changing **one character at a time** (`a` → `z`)

---

## 🧠 Strategy (BFS)

1. Put all words into a `HashSet` for `O(1)` lookup
2. If `dest` is not in the set → impossible → return `0`
3. Start BFS from `src`
4. For each word:

   * Change one character at a time
   * If the new word exists in the set:

     * Add it to the queue
     * Remove it from the set (mark visited)
5. Count levels → the level where `dest` is found is the answer

---

## ✅ Java Solution (BFS)

```java
import java.util.*;

public class Solution {
  public static int ladderLength(String src, String dest, List<String> words) {
    Set<String> dict = new HashSet<>(words);
    if (!dict.contains(dest)) return 0;

    Queue<String> q = new LinkedList<>();
    q.offer(src);
    int level = 1;

    while (!q.isEmpty()) {
      int size = q.size();

      for (int i = 0; i < size; i++) {
        String curr = q.poll();

        if (curr.equals(dest)) return level;

        char[] arr = curr.toCharArray();
        for (int j = 0; j < arr.length; j++) {
          char original = arr[j];

          for (char c = 'a'; c <= 'z'; c++) {
            if (c == original) continue;

            arr[j] = c;
            String next = new String(arr);

            if (dict.contains(next)) {
              q.offer(next);
              dict.remove(next); // mark visited
            }
          }

          arr[j] = original;
        }
      }

      level++;
    }

    return 0;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(N × L × 26)`
  * `N` = number of words
  * `L` = word length

* **Space Complexity:** `O(N)`
