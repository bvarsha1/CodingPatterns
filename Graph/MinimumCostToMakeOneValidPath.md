## Minimum Cost to Make at Least One Valid Path in a Grid

#### Statement

You are given an
`m × n` grid, where each cell contains a directional sign indicating which neighboring cell to move to next. The sign in a cell `grid[i][j]` can be:

* `1`: Move right, i.e., from `grid[i][j]` to `grid[i][j + 1]`
* `2`: Move left, i.e., from `grid[i][j]` to `grid[i][j - 1]`
* `3`: Move down, i.e., from `grid[i][j]` to `grid[i + 1][j]`
* `4`: Move up, i.e., from `grid[i][j]` to `grid[i - 1][j]`

> **Note:** Some signs may point outside the boundaries of the grid.

Your starting position is the top-left cell `(0, 0)`.
A valid path is any sequence of moves beginning at `(0, 0)` and ending at the bottom-right cell `(m − 1, n − 1)`, where each move follows the direction of the sign in the current cell.

You are allowed to change the direction of a sign in any cell, but each modification incurs a cost of `1`, and each sign can be modified only once.

Your task is to determine the **minimum total cost** required to ensure that **at least one valid path exists** from the top-left to the bottom-right cell.

---

#### ✔️ Constraints

* `n = grid.length`
* `m = grid[i].length`
* `1 ≤ m, n ≤ 50`
* `1 ≤ grid[i][j] ≤ 4`

---

## 🎯 Intuition

This is a **shortest path problem with costs**:

* Moving in the direction already indicated by a cell costs `0`
* Changing the direction to move somewhere else costs `1`

Key observations:

* Each cell is a node in a graph
* From each cell, you can move to **up to 4 neighbors**
* The **edge weight** is:

  * `0` if the move follows the current sign
  * `1` if the sign must be changed

Because all edge weights are either `0` or `1`, we can use **0–1 BFS** (a specialized BFS using a deque) to efficiently find the minimum cost.

Why not plain BFS?

* BFS assumes all edges have equal weight
* Here, edges have weight `0` or `1`

Why not Dijkstra?

* It works, but 0–1 BFS is faster and simpler for `{0,1}` weights

---

## 🧠 Approach (0–1 BFS)

1. Treat each cell `(i, j)` as a node
2. Maintain a `dist[i][j]` = minimum cost to reach that cell
3. Use a **deque**:

   * If moving costs `0` → push to front
   * If moving costs `1` → push to back
4. Start from `(0, 0)` with cost `0`
5. Continue until `(m−1, n−1)` is reached with minimum cost

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {

    // Directions: right, left, down, up
    private static final int[][] DIRS = {
        {0, 1},   // right
        {0, -1},  // left
        {1, 0},   // down
        {-1, 0}   // up
    };

    public int minCost(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;

        int[][] dist = new int[m][n];
        for (int[] row : dist) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }

        Deque<int[]> dq = new ArrayDeque<>();
        dq.offerFirst(new int[]{0, 0});
        dist[0][0] = 0;

        while (!dq.isEmpty()) {
            int[] curr = dq.pollFirst();
            int r = curr[0], c = curr[1];

            for (int d = 0; d < 4; d++) {
                int nr = r + DIRS[d][0];
                int nc = c + DIRS[d][1];

                if (nr < 0 || nc < 0 || nr >= m || nc >= n) continue;

                // cost is 0 if direction matches, else 1
                int cost = (grid[r][c] == d + 1) ? 0 : 1;

                if (dist[r][c] + cost < dist[nr][nc]) {
                    dist[nr][nc] = dist[r][c] + cost;
                    if (cost == 0) {
                        dq.offerFirst(new int[]{nr, nc});
                    } else {
                        dq.offerLast(new int[]{nr, nc});
                    }
                }
            }
        }

        return dist[m - 1][n - 1];
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(m × n)`
  * Each cell is processed at most a few times
* **Space Complexity:** `O(m × n)`
  * Distance matrix + deque
