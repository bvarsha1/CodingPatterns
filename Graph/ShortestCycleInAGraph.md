## Shortest Cycle in a Graph

#### Statement

You are given a bidirectional graph with n vertices, labeled from `0` to `n - 1`. The graph is represented by a 2D integer array `edges`, where each element `edges[i] = [ui, vi]` represents an edge connecting vertex `ui` and vertex `vi`. Each vertex pair has at most one edge between them, and no vertex is connected to itself.

Your task is to find the length of the shortest cycle in the graph. A cycle is defined as a path that starts and ends at the same vertex, with each edge in the path appearing exactly once. If no cycle exists in the graph, return `-1`.

---

#### ✔️ Constraints

* `2 ≤ n ≤ 1000`
* `1 ≤ edges.length ≤ 1000`
* `edges[i].length = 2`
* `0 ≤ ui, vi < n`
* `ui != vi`
* No duplicate edges exist in the graph

---

## 🎯 Intuition

Since the graph is **undirected**, the shortest cycle can be efficiently detected using **Breadth-First Search (BFS)**.

Key ideas:

* A cycle is formed when, during BFS, we encounter a **previously visited node that is not the parent**
* BFS explores nodes level by level, so the **first time** we detect a cycle, it is guaranteed to be the **shortest cycle involving that BFS source**
* Because the graph may be disconnected, we must run BFS from **each unvisited node**
* We track:

  * `dist[]` → distance from the BFS start node
  * `parent[]` → to avoid falsely detecting the immediate parent edge as a cycle

### Why this formula works

```
cycle length = dist[curr] + dist[nbr] + 1
```

Explanation:

* `dist[curr]` = distance from source to current node
* `dist[nbr]` = distance from source to neighbor
* `+1` = the edge directly connecting `curr` and `nbr`

This gives the total number of edges in the cycle.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {

    public int findShortestCycle(int n, int[][] edges) {
        // Build adjacency list
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }

        for (int[] e : edges) {
            graph.get(e[0]).add(e[1]);
            graph.get(e[1]).add(e[0]);
        }

        boolean[] visited = new boolean[n];
        int minLen = Integer.MAX_VALUE;

        // Run BFS from every unvisited node (handle disconnected graph)
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                minLen = Math.min(minLen, bfs(graph, i, visited));
            }
        }

        return (minLen == Integer.MAX_VALUE) ? -1 : minLen;
    }

    public static int bfs(List<List<Integer>> graph, int node, boolean[] visited) {
        int n = visited.length;
        int[] dist = new int[n];
        int[] parent = new int[n];
        Arrays.fill(dist, -1);
        Arrays.fill(parent, -1);

        Queue<Integer> q = new ArrayDeque<>();
        dist[node] = 0;
        q.offer(node);

        int minLen = Integer.MAX_VALUE;

        while (!q.isEmpty()) {
            int curr = q.poll();
            visited[curr] = true;

            for (int nbr : graph.get(curr)) {
                if (dist[nbr] == -1) {
                    dist[nbr] = dist[curr] + 1;
                    parent[nbr] = curr;
                    q.offer(nbr);
                } 
                // visited neighbor that is NOT parent → cycle
                else if (parent[curr] != nbr) {
                    minLen = Math.min(minLen, dist[curr] + dist[nbr] + 1);
                }
            }
        }

        return minLen;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n + edges.length)`
* **Space Complexity:** `O(n + edges.length)`
