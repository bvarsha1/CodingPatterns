## Path with Maximum Probability

#### Statement

You are given an undirected weighted graph of `n` nodes, represented by a 0-indexed list, `edges`, where
`edges[i] = [a, b]` is an undirected edge connecting the nodes `a` and `b`.
There is another list `succProb`, where `succProb[i]` is the probability of success of traversing the edge `edges[i]`.

Additionally, you are given two nodes, `start` and `end`. Your task is to find the path with the **maximum probability of success** to go from `start` to `end` and return its success probability.
If there is no path from `start` to `end`, return `0`.

---

#### ✔️ Constraints

* `2 ≤ n ≤ 10³`
* `0 ≤ start, end < n`
* `start != end`
* `0 ≤ a, b < n`
* `a != b`
* `0 ≤ succProb.length == edges.length ≤ 2 × 10³`
* `0 ≤ succProb[i] ≤ 1`
* There is, at most, one edge between every two nodes.

---

## 🎯 Intuition

This problem is a **shortest-path variant**, but instead of:

* **minimizing distance**

we are:

* **maximizing probability**

Key observations:

* The probability of a path is the **product** of probabilities of its edges.
* We want the path from `start` to `end` with the **maximum product**.
* This is similar to **Dijkstra’s algorithm**, but:

  * Instead of choosing the minimum distance so far,
  * We choose the **maximum probability so far**.

### Why Dijkstra-style works here?

* All probabilities are in the range `[0, 1]`
* Multiplying probabilities **never increases** the value
* Once we reach a node with the maximum possible probability, there is no better way to reach it later

So we can safely use:

* A **max-heap (priority queue)**
* Greedily expand the path with the highest probability first

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static double maxProbability(
            int n,
            int[][] edges,
            double[] succProb,
            int start,
            int end
    ) {
        // Build graph
        Map<Integer, List<double[]>> graph = new HashMap<>();

        for (int i = 0; i < edges.length; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            double p = succProb[i];

            graph.putIfAbsent(u, new ArrayList<>());
            graph.putIfAbsent(v, new ArrayList<>());

            graph.get(u).add(new double[]{v, p});
            graph.get(v).add(new double[]{u, p});
        }

        // Max heap based on probability
        PriorityQueue<double[]> pq =
                new PriorityQueue<>((a, b) -> Double.compare(b[1], a[1]));

        double[] maxProb = new double[n];
        maxProb[start] = 1.0;

        pq.offer(new double[]{start, 1.0});

        while (!pq.isEmpty()) {
            double[] curr = pq.poll();
            int node = (int) curr[0];
            double prob = curr[1];

            if (node == end) return prob;

            if (prob < maxProb[node]) continue;

            for (double[] nbr : graph.getOrDefault(node, new ArrayList<>())) {
                int next = (int) nbr[0];
                double newProb = prob * nbr[1];

                if (newProb > maxProb[next]) {
                    maxProb[next] = newProb;
                    pq.offer(new double[]{next, newProb});
                }
            }
        }

        return 0.0;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(E log V)`
* **Space Complexity:** `O(E + V)`