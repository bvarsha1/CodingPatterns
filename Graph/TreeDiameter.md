## Tree Diameter

#### Statement

Given an undirected tree with `n` nodes labeled from `0` to `n − 1`, represented by a 2D array `edges` where
`edges.length == n − 1`. Each `edges[i] = [ai, bi]` denotes an undirected edge between nodes `ai` and `bi`. Your task is to return the diameter of the tree.

> The diameter of a tree is the number of edges in the longest path between any two nodes.

---

#### ✔️ Constraints

* `n == edges.length + 1`
* `edges[i].length = 2`
* `1 <= n <= 10^3`
* `0 <= ai, bi < n`
* `ai != bi`

---

## 🎯 Intuition

A **tree** has no cycles and exactly one path between any two nodes.

A key property of trees:

> If you pick **any node** and find the **farthest node** from it, then start again from that farthest node and find the farthest node from there — the distance between these two nodes is the **tree diameter**.

This works because:

* The longest path in a tree must start and end at two leaf-like extremities.
* The first BFS/DFS takes us to one end of the diameter.
* The second BFS/DFS measures the full diameter.

So the approach is:

1. Build an adjacency list
2. Run BFS/DFS from node `0` to find the farthest node `u`
3. Run BFS/DFS from `u` to find the maximum distance → **diameter**

---

## ✅ Java Solution (BFS-based)

```java
import java.util.*;

public class Solution {
    public int treeDiameter(int[][] edges) {
        if(edges.length == 0) return 0;
        
        int n = edges.length + 1;
        HashMap<Integer, List<Integer>> graph = new HashMap<>();
        
        for(int[] e : edges) {
            graph.putIfAbsent(e[0], new ArrayList<>());
            graph.putIfAbsent(e[1], new ArrayList<>());
            graph.get(e[0]).add(e[1]);
            graph.get(e[1]).add(e[0]);
        }
        
        int farthest = bfsFindFarthest(0, graph)[0];
        
        int diameter = bfsFindFarthest(farthest, graph)[1];
        
        return diameter;
    }
    
    public int[] bfsFindFarthest(int start, HashMap<Integer, List<Integer>> graph) {
        int n = graph.size();
        boolean[] visited = new boolean[n];
        int[] dist = new int[n];
        Queue<Integer> q = new ArrayDeque<>();
        int farthest = start;
        
        q.offer(start);
        dist[start] = 0;
        visited[start] = true;
        
        while(!q.isEmpty()) {
            int curr = q.poll();
            farthest = curr;
            
            for(int nbr : graph.get(curr)) {
                if(!visited[nbr]) {
                    visited[nbr] = true;
                    dist[nbr] = dist[curr] + 1;
                    q.offer(nbr);
                }
            }
        }
        
        return new int[]{farthest, dist[farthest]};
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(n)`