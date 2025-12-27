## Network Delay Time

#### Statement

A network of n nodes labeled `1` to `n` is provided along with a list of travel times for directed edges represented as
`times[i] = (xi, yi, ti)`, where `xi` is the source node, `yi` is the target node, and `ti` is the delay time from the source node to the target node.

Considering we have a starting node, `k`, we have to determine the minimum time required for all the remaining `n − 1` nodes to receive the signal. Return `-1` if it’s not possible for all `n − 1` nodes to receive the signal.

---

#### ✔️ Constraints

* `1 ≤ k ≤ n ≤ 100`
* `1 ≤ times.length ≤ 6000`
* `times[i].length == 3`
* `1 ≤ x, y ≤ n`
* `x != y`
* `0 ≤ t ≤ 100`
* Unique pairs of `(x, y)`, which means that there should be no multiple edges

---

## 🎯 Intuition

This is a **single-source shortest path problem** on a **directed weighted graph**.

Key observations:

* We start from node `k`
* Each edge has a **non-negative weight**
* We want the **minimum time** for the signal to reach **every node**

This naturally fits **Dijkstra’s algorithm**:

* Use a **priority queue (min-heap)** to always expand the node with the smallest current time
* Keep track of visited nodes to avoid reprocessing
* Track the **maximum time** taken among all reachable nodes
* If all `n` nodes are visited → return the maximum time
* Otherwise → return `-1`

---

## ✅ Java Solution

```java
import java.util.*;

class NetworkDelay {
  public static int networkDelayTime(int[][] times, int n, int k) {
    HashMap<Integer, List<int[]>> graph = createGraph(times);

    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
    pq.offer(new int[] {0, k});

    Set<Integer> visited = new HashSet<>();
    int timeTaken = 0;

    while (!pq.isEmpty()) {
      int[] curr = pq.poll();
      int time = curr[0];
      int node = curr[1];

      if (visited.contains(node)) continue;

      // process node
      visited.add(node);
      timeTaken = Math.max(timeTaken, time);

      List<int[]> neighbors = graph.getOrDefault(node, new ArrayList<>());
      for (int[] nbr : neighbors) {
        if (!visited.contains(nbr[0])) {
          pq.offer(new int[] {time + nbr[1], nbr[0]});
        }
      }
    }

    return visited.size() == n ? timeTaken : -1;
  }

  private static HashMap<Integer, List<int[]>> createGraph(int[][] times) {
    HashMap<Integer, List<int[]>> graph = new HashMap<>();

    for (int[] t : times) {
      int src = t[0], dest = t[1], time = t[2];
      graph.computeIfAbsent(src, k -> new ArrayList<>())
           .add(new int[] {dest, time});
    }

    return graph;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(E log V)`
  where `E = times.length` and `V = n`
* **Space Complexity:** `O(V + E)`

---

📌 **Why Dijkstra works here:**
All edge weights are **non-negative**, and we need the shortest time from a single source to all nodes.