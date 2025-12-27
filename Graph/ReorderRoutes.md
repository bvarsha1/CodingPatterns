## Reorder Routes to Make All Paths Lead to the City Zero

#### Statement

There are `n` cities labeled from `0` to `n − 1`, connected by `n − 1` roads, so there is only one route between any two cities. This road network forms a tree structure.

Last year, the Ministry of transport made all roads one-way due to their narrow width. These roads are represented as `connections`, where each entry
`connections[i] = [ai, bi]` means there is a road going from city `ai` to city `bi`.

This year, a major event will occur in the capital city (city `0`), and people from all other cities must reach it. Your task is to reorient some roads so every city has a valid path leading to city `0`. Return the **minimum number of roads** that must be changed to achieve this.

> **Note:** A solution is guaranteed to exist, meaning that every city can eventually reach city `0` after reordering.

---

#### ✔️ Constraints

* `2 ≤ n ≤ 5 * 10^4`
* `connections.length == n − 1`
* `connections[i].length == 2`
* `0 ≤ ai, bi ≤ n − 1`

---

## 🎯 Intuition

Key observations:

* The graph is a **tree**, so there is exactly **one unique path** between any two cities.
* Ultimately, **all paths must lead toward city `0`**.
* Some roads already point in the correct direction (toward `0`), while others point **away** and must be reversed.

### Strategy

1. Treat the tree as an **undirected graph for traversal**, but keep track of the **original direction** of each road.
2. Start a **DFS or BFS from city `0`**.
3. For every neighboring city:

   * If the original road goes **away from `0`**, it must be reversed → count it.
   * If it already goes **toward `0`**, no change needed.
4. Visit all cities exactly once.

Because the graph is a tree:

* Every edge is checked once
* Each incorrect direction is counted exactly once

---

## ✅ Java Solution (DFS)

```java
import java.util.*;

public class Solution {
    public int minReorder(int n, int[][] connections) 
    {
        List<List<int[]>> graph = new ArrayList<>();
        for(int i = 0; i < n; i++)
            graph.add(i, new ArrayList<>());
            
        for(int[] c : connections) {
            graph.get(c[0]).add(new int[] { c[1], 1 }); // original, may need to be reversed
            graph.get(c[1]).add(new int[] { c[0], 0 }); // reversed
        }
        
        boolean[] visited = new boolean[n];
        int ans = dfs(0, graph, visited);
        
        return ans;
    }
    
    public int dfs(int city, List<List<int[]>> graph, boolean[] visited) {
        visited[city] = true;
        int reversed = 0;
        
        for(int[] conn : graph.get(city)) {
            int nbr = conn[0];
            if(!visited[nbr]) {
                reversed += conn[1];
                reversed += dfs(nbr, graph, visited);
            }
        }
        
        return reversed;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(n)` (adjacency list + visited array)
