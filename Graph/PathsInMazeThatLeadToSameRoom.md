## Paths in Maze That Lead to Same Room

#### Statement

A maze consists of `n` rooms numbered from `1 − n`, and some rooms are connected by corridors. You are given a 2D integer array, `corridors`, where
`corridors[i] = [room1, room2]` indicates that there is a corridor connecting `room1` and `room2`, allowing a person in the maze to go from `room1` to `room2` and vice versa.

The designer of the maze wants to know how confusing the maze is. The **confusion score** of the maze is the number of different **cycles of length 3**.

For example:

* `1 → 2 → 3 → 1` is a cycle of length `3`
* `1 → 2 → 3 → 4` is **not**
* `1 → 2 → 3 → 2 → 1` is **not**

Two cycles are considered to be different if one or more of the rooms visited in the first cycle is not in the second cycle.

Return the confusion score of the maze.

---

#### ✔️ Constraints

* `2 ≤ n ≤ 100`
* `1 ≤ corridors.length ≤ 5 × 10²`
* `corridors[i].length = 2`
* `1 ≤ room1ᵢ, room2ᵢ ≤ n`
* `room1ᵢ ≠ room2ᵢ`
* There are no duplicate corridors.

---

## 🎯 Intuition

A **cycle of length 3** exists if:

```
roomA ↔ roomB
roomB ↔ roomC
roomC ↔ roomA
```

Key observation:

* If two rooms `u` and `v` share a **common neighbor `w`**, then `(u, v, w)` forms a triangle.
* While processing each corridor `(u, v)`, we can count how many **common neighbors** they already have.
* Each common neighbor contributes **one unique 3-cycle**.

To implement this efficiently:

* Maintain an adjacency list using `HashSet` for fast lookup
* For every corridor, compute the intersection of neighbors
* Accumulate the count

This avoids triple nested loops and works efficiently within constraints.

---

## ✅ Java Solution

```java
import java.util.*;

public class PathsInMaze {
    public static int numberOfPaths(int n, int[][] corridors) {
        Map<Integer, Set<Integer>> nbrs = new HashMap<>();
        int count = 0;

        for (int[] cor : corridors) {
            int r1 = cor[0];
            int r2 = cor[1];

            nbrs.putIfAbsent(r1, new HashSet<>());
            nbrs.putIfAbsent(r2, new HashSet<>());

            // add the undirected edge
            nbrs.get(r1).add(r2);
            nbrs.get(r2).add(r1);

            // count common neighbors before adding the edge
            count += nbrIntersection(nbrs.get(r1), nbrs.get(r2));
        }

        return count;
    }

    private static int nbrIntersection(Set<Integer> s1, Set<Integer> s2) {
        int cnt = 0;
        for (int node : s1) {
            if (s2.contains(node)) cnt++;
        }
        return cnt;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(E · d)`
  where `E` is the number of corridors and `d` is the average degree of a node
* **Space Complexity:** `O(n + E)`
