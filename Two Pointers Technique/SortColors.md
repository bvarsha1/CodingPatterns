## Sort Colors (Dutch National Flag Problem)

You are given an array `colors` consisting only of:

- `0` → Red  
- `1` → White  
- `2` → Blue  

Sort the array **in-place** so that all `0`s come first, then `1`s, then `2`s.

**No built-in sorting functions allowed.**  
**No extra space should be used.**

---

### Constraints

- `1 ≤ colors.length ≤ 300`
- `colors[i] ∈ {0, 1, 2}`

---

## Intuition

This is a classic **Dutch National Flag** problem.

We divide the array into 3 regions:

| Region | Meaning |
|--------|---------|
| left side | All 0s |
| middle | All 1s |
| right side | All 2s |

Use **three pointers**:
- `start`  → next position for 0  
- `curr`  → current element we are checking  
- `end` → next position for 2  

We reorder the array as we scan it once.

---

## Java Solution
#### Bucket Sort or counting sort
``` java
import java.util.*;

public class Solution {
    public static int[] sortColors (int[] colors) {
        int[] colorFreq = new int[3];
    
        // Count frequency of each color
        for (int c : colors) {
            colorFreq[c]++;
        }
    
        int idx = 0;
        for (int c = 0; c < 3; c++) {
            while (colorFreq[c] > 0) {
                colors[idx++] = c;
                colorFreq[c]--;
            }
        }
        return colors;
    }
}
```
- Time Complexity: O(n)
- Space Complexity: O(1) - constant space of 3

#### Two pointer (Quicksort partitioning) approach
```java
import java.util.*;

public class Solution {
    public static int[] sortColors (int[] colors) {
        int s = 0, e = colors.length - 1;
        int curr = 0;
        while(curr <= e) {
          if(colors[curr] == 0) {
            swap(colors, s, curr);
            s++;
            curr++;
          } else if(colors[curr] == 1) {
            curr++;
          } else {
            swap(colors, curr, e);
            e--;
          }
        }
        return colors;
    }
    
    public static void swap(int[] a, int i, int j) {
      int temp = a[i];
      a[i] = a[j];
      a[j] = temp;
    }
}
```
- Time Complexity: O(n)
- Space Complexity: O(1)