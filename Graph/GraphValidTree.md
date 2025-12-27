## Graph Valid Tree

#### Statement

Given `n` as the number of nodes and an array of the edges of a graph, find out if the graph is a valid tree. The nodes of the graph are labeled from `0` to `n − 1`, and
`edges[i] = [x, y]` represents an undirected edge connecting the nodes `x` and `y` of the graph.

A graph is a valid tree when all the nodes are connected and there is no cycle between them.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 1000`
* `0 ≤ edges.length ≤ 2000`
* `edges[i].length = 2`
* `0 ≤ x, y < n`
* `x != y`
* There are no repeated edges.

---

## 🎯 Intuition

A graph is a **valid tree** if and only if:

1. **It is connected** → every node is reachable from any starting node
2. **It has no cycles** → there is no alternate path that revisits a node

To check this:

* Build an **adjacency list** representation of the graph
* Perform a **DFS traversal**
* Track visited nodes
* Detect cycles using a `parent` pointer:

  * If we encounter a visited node that is **not the parent**, a cycle exists
* After DFS:

  * If all `n` nodes are visited **and** no cycle was found → valid tree

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static boolean validTree(int n, int[][] edges) {
        Set<Integer> visited = new HashSet<>();
        List<List<Integer>> graph = createGraph(n, edges);

        // check for cycle starting from node 0
        boolean graphContainsCycle = containsCycle(graph, 0, visited, -1);

        // valid tree = connected + no cycle
        return visited.size() == n && !graphContainsCycle;
    }

    public static List<List<Integer>> createGraph(int n, int[][] edges) {
        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            graph.add(new LinkedList<>());
        }

        for (int[] e : edges) {
            graph.get(e[0]).add(e[1]);
            graph.get(e[1]).add(e[0]);
        }

        return graph;
    }

    public static boolean containsCycle(
            List<List<Integer>> graph,
            int node,
            Set<Integer> visited,
            int parent
    ) {
        visited.add(node);

        for (int nbr : graph.get(node)) {
            if (!visited.contains(nbr)) {
                if (containsCycle(graph, nbr, visited, node)) {
                    return true;
                }
            } else if (nbr != parent) {
                // visited neighbor not equal to parent → cycle
                return true;
            }
        }

        return false;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(n + edges.length)
* **Space Complexity:** O(n + edges.length)