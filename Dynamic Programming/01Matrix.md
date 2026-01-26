## 01 Matrix

#### Statement

Given an *m × n* binary matrix, `mat`, find the distance from each cell to the nearest 0. The distance between two adjacent cells is 1. Cells to the left, right, above, and below the current cell will be considered adjacent.

---

#### ✔️ Constraints

- 1 ≤ `mat.row` , `mat.col`≤ 50
- 1 ≤ `mat.row * mat.col` ≤ 2500
- `mat[i][j]` ∈ {0,1}
- There is at least one 00 in `mat`.

---

## 🎯 Intuition

We want the minimum distance from each cell to the nearest `0`.

Key idea:

* Cells with `0` already have distance `0`
* Cells with `1` must find nearest `0`

There are two clean approaches:

### 🧠 Approach 1: Multi-Source BFS (Shortest Path)

1. Put all `0` cells into a queue with distance `0`
2. Mark all `1` cells as infinity initially
3. BFS gradually assigns distances to neighbors

This ensures shortest distance because BFS expands layer-by-layer.

### 🧠 Approach 2: Dynamic Programming (Two-Pass DP)

If BFS isn't allowed or desired, we can use DP:

1. Initialize `dist[i][j] = 0` if `mat[i][j] == 0`, else `∞`
2. First pass (top-left → bottom-right):

   * Check top and left neighbors
3. Second pass (bottom-right → top-left):

   * Check bottom and right neighbors

Use recurrence:

```
dist[i][j] = min(
    dist[i][j],
    dist[i-1][j] + 1,
    dist[i][j-1] + 1,
    dist[i+1][j] + 1,
    dist[i][j+1] + 1
)
```

Both passes combined ensure minimal distances propagate in all directions.

---

## ✅ Java Solution

### BFS Solution

```java
import java.util.*;

class Matrix01 {
    public int[][] updateMatrix(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int[][] dist = new int[m][n];
        Queue<int[]> q = new LinkedList<>();

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                    dist[i][j] = 0;
                    q.offer(new int[]{i, j});
                } else {
                    dist[i][j] = Integer.MAX_VALUE;
                }
            }
        }

        int[][] dirs = {{1,0}, {-1,0}, {0,1}, {0,-1}};

        while (!q.isEmpty()) {
            int[] cell = q.poll();
            int r = cell[0], c = cell[1];

            for (int[] d : dirs) {
                int nr = r + d[0], nc = c + d[1];
                if (nr >= 0 && nr < m && nc >= 0 && nc < n) {
                    if (dist[nr][nc] > dist[r][c] + 1) {
                        dist[nr][nc] = dist[r][c] + 1;
                        q.offer(new int[]{nr, nc});
                    }
                }
            }
        }
        return dist;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(m × n)
* **Space Complexity:** O(m × n)

<br>

### DP - Two-Pass & In-Place Solution

```java
public class UpdateMatrix {
  public static int[][] updateMatrix(int[][] mat) {
    int rl = mat.length, cl = mat[0].length;
    
    int minNbr = rl * cl;
    for(int i = 0; i < rl; i++) {
      for(int j = 0; j < cl; j++) {
        if(mat[i][j] != 0) {
          mat[i][j] = 1 + Math.min(
                            (i > 0) ? mat[i - 1][j] : minNbr,
                            (j > 0) ? mat[i][j - 1] : minNbr
                          );
        }
      }
    }
    
    for(int i = rl - 1; i >= 0; i--) {
      for(int j = cl - 1; j >= 0; j--) {
        if(mat[i][j] != 0) {
          mat[i][j] = Math.min(
                        mat[i][j],
                        1 + Math.min(
                            (i < rl - 1) ? mat[i + 1][j] : minNbr,
                            (j < cl - 1) ? mat[i][j + 1] : minNbr
                        )
                      );
        }
      }
    }
    
    return mat;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(m × n)
* **Space Complexity:** O(1) extra (besides output grid)