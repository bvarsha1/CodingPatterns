
## Closest Node to Path in Tree

#### Statement

You are given a positive integer, n, representing the number of nodes in a tree, numbered from `0` to `n−1`. You are also given a 2D integer array `edges` of length `n−1`, where `edges[i] = [ui, vi]` indicates that there is a bidirectional edge connecting nodes `ui` and `vi`.

You are also given a 2D integer array `query` of length `m`, where `query[i] = [starti, endi, nodei]`.

For each query `i`, find the node on the path between `starti` and `endi` that is closest to `nodei` in terms of the number of edges.

Return an integer array where the value at index `i` corresponds to the answer for the `i`-th query.

> **Note:** If there are multiple such nodes at the same minimum distance, return the one with the smallest index.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 1000`
* `edges.length == n − 1`
* `edges[i].length == 2`
* `0 ≤ ui, vi ≤ n − 1`
* `ui != vi`
* `1 ≤ query.length ≤ 1000`
* `query[i].length == 3`
* `0 ≤ starti, endi, nodei ≤ n − 1`
* The graph is a tree

---

## 🎯 Intuition

This problem combines **tree paths** and **shortest distances**.

For each query `(start, end, node)`:

1. We care **only about nodes on the path** from `start` to `end`.
2. Among those nodes, we want the one with the **minimum distance to `node`**.
3. Distance is measured by **number of edges**.
4. If multiple nodes have the same minimum distance → pick the **smallest index**.

---

### Key Observations

* Since the graph is a **tree**, there is exactly **one unique path** between any two nodes.
* The simplest way (given small constraints) is:

  1. Explicitly find the path from `start` to `end`
  2. For every node on that path, compute its distance to `node`
  3. Pick the best one

Given `n ≤ 1000` and `queries ≤ 1000`, this brute-force DFS/BFS-based approach is acceptable.

---

## 🧠 Strategy

### Step 1: Build the adjacency list

Convert `edges` into a graph representation.

### Step 2: For each query

* Use DFS to **extract the path** from `start` to `end`
* Run BFS from `node` to compute distances to all nodes
* Iterate over the path:

  * Track the node with minimum distance
  * Break ties using smaller index

---

## ✅ Java Solution (DFS for path + BFS for distance)

```java
import java.util.*;

class Solution {

    public int[] closestNode(int n, int[][] edges, int[][] query) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

        // build tree
        for (int[] e : edges) {
            graph.get(e[0]).add(e[1]);
            graph.get(e[1]).add(e[0]);
        }

        int[] ans = new int[query.length];

        for (int i = 0; i < query.length; i++) {
            int start = query[i][0];
            int end = query[i][1];
            int node = query[i][2];

            // Step 1: get path from start to end
            List<Integer> path = new ArrayList<>();
            dfsPath(start, -1, end, graph, path);

            // Step 2: compute distance from node to all others
            int[] dist = bfsDistance(node, graph, n);

            // Step 3: find closest node on path
            int bestNode = -1;
            int bestDist = Integer.MAX_VALUE;

            for (int p : path) {
                if (dist[p] < bestDist || 
                   (dist[p] == bestDist && p < bestNode)) {
                    bestDist = dist[p];
                    bestNode = p;
                }
            }

            ans[i] = bestNode;
        }

        return ans;
    }

    // DFS to find path from src to dest
    private boolean dfsPath(
        int curr,
        int parent,
        int dest,
        List<List<Integer>> graph,
        List<Integer> path
    ) {
        path.add(curr);
        if (curr == dest) return true;

        for (int nei : graph.get(curr)) {
            if (nei != parent) {
                if (dfsPath(nei, curr, dest, graph, path)) {
                    return true;
                }
            }
        }

        path.remove(path.size() - 1);
        return false;
    }

    // BFS to compute distances from source
    private int[] bfsDistance(int src, List<List<Integer>> graph, int n) {
        int[] dist = new int[n];
        Arrays.fill(dist, -1);

        Queue<Integer> q = new ArrayDeque<>();
        q.offer(src);
        dist[src] = 0;

        while (!q.isEmpty()) {
            int curr = q.poll();
            for (int nei : graph.get(curr)) {
                if (dist[nei] == -1) {
                    dist[nei] = dist[curr] + 1;
                    q.offer(nei);
                }
            }
        }

        return dist;
    }
}
```

---

#### ⏱ Complexity Analysis

For each query:
* DFS to find path: `O(n)`
* BFS for distances: `O(n)`
* Path scan: `O(n)`

**Overall Complexity**

* **Time Complexity:** `O(m × n)`

* **Space Complexity:** `O(n)`
  * adjacency list
  * BFS queue
  * recursion stack
