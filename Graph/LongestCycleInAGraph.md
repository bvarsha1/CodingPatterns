## Longest Cycle in a Graph

#### Statement

You are given a directed graph with n nodes, labeled from `0` to `n - 1`. Each node in the graph has at most one outgoing edge.

The graph is described using a 0-indexed integer array `edges` of length `n`, where:

* `edges[i]` represents a directed edge from node `i` to node `edges[i]`
* If node `i` has no outgoing edge, then `edges[i] == -1`

Your task is to find the longest cycle length in the graph. If no cycle exists, return `-1`.

> **Note:** A cycle is defined as a path that starts and ends at the same node, following the direction of the edges.

---

#### ✔️ Constraints

* `n == edges.length`
* `2 ≤ n ≤ 10^5`
* `-1 ≤ edges[i] ≤ n - 1`
* `edges[i] != i`

---

## 🎯 Intuition

Key observations:

* Each node has **at most one outgoing edge**
  → From any node, there is **only one possible path forward**
* Cycles can only form by **revisiting a node within the same traversal**
* Once a node is fully processed, we **never need to start from it again**

Strategy:

* Maintain a global `visited[]` array to mark nodes that are already processed
* For each unvisited node:

  * Walk forward following outgoing edges
  * Track the **time (step index)** at which each node is first visited in this traversal using `visitTime[]`
* If we encounter:

  * `-1` → no cycle
  * a node already visited **in the current traversal** → cycle detected

Cycle length is calculated as:

```
currentTime - visitTime[cycleStartNode]
```

We take the maximum over all detected cycles.

---

## ✅ Java Solution (Your Approach)

```java
public class Solution {
    public int longestCycle(int[] edges) {
        int n = edges.length;
        boolean[] visited = new boolean[n];
        int maxLen = -1;

        for (int i = 0; i < n; i++) {
            // Skip if this node is already fully processed
            if (visited[i]) continue;

            int curr = i;
            int time = 0;
            Integer[] visitTime = new Integer[n];

            // Traverse until we hit -1 or a visited node
            while (curr != -1 && !visited[curr]) {
                visitTime[curr] = time++;
                visited[curr] = true;
                curr = edges[curr];
            }

            // If curr is not -1 and was visited in THIS traversal → cycle
            if (curr != -1 && visitTime[curr] != null) {
                maxLen = Math.max(maxLen, time - visitTime[curr]);
            }
        }

        return maxLen;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(n)`
