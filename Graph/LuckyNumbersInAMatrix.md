## Lucky Numbers in a Matrix

#### Statement

Given an
`m × n` matrix of distinct numbers, return the lucky number in the matrix.

> A lucky number is an element of the matrix such that it is the **smallest element in its row** and **largest in its column**.

---

#### ✔️ Constraints

* `m = matrix.length`
* `n = matrix[i].length`
* `1 ≤ n, m ≤ 50`
* `1 ≤ matrix[i][j] ≤ 10^5`
* All elements in the matrix are distinct.

---

## 🎯 Intuition

A number is **lucky** if it satisfies **two conditions at the same time**:

1. It is the **minimum in its row**
2. It is the **maximum in its column**

So the idea is straightforward:

* First, find the **minimum value of each row**
* Then, find the **maximum value of each column**
* Finally, check if any value appears in **both**:

  * row minimums
  * column maximums

Because all elements are distinct, there can be **at most one lucky number**.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static List<Integer> luckyNumbers(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;

        int[] rowMin = new int[m];
        int[] colMax = new int[n];

        // initialize row minimums
        Arrays.fill(rowMin, Integer.MAX_VALUE);
        // initialize column maximums
        Arrays.fill(colMax, Integer.MIN_VALUE);

        // compute row minimums and column maximums
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                rowMin[i] = Math.min(rowMin[i], matrix[i][j]);
                colMax[j] = Math.max(colMax[j], matrix[i][j]);
            }
        }

        List<Integer> result = new ArrayList<>();

        // check for lucky numbers
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == rowMin[i] && matrix[i][j] == colMax[j]) {
                    result.add(matrix[i][j]);
                }
            }
        }

        return result;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(m × n)`
* **Space Complexity:** `O(m + n)`