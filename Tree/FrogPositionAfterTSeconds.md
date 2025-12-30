
## Frog Position After T Seconds

#### Statement

You are given an undirected tree with `n` vertices labeled from `1` to `n`. A frog starts at vertex `1`, at time `0`, and makes one move per second.

At each step, the frog follows these rules:

* **Move to an unvisited neighbor:**
  If the frog has unvisited neighbors, it jumps to one of them, chosen uniformly at random.

* **No revisiting:**
  The frog cannot jump back to a vertex it has already visited.

* **Stay when stuck:**
  If the frog has no unvisited neighbors, it stays at its current vertex.

The tree is represented as an array `edges`, where `edges[i] = [ai, bi]` means there is an edge between vertices `ai` and `bi`.

Your task is to return the probability that, after `t` seconds, the frog is on the vertex `target`.

> **Note:** Your answer will be accepted if its absolute difference from the actual value is at most `10^-5`.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 100`
* `edges.length == n - 1`
* `edges[i].length == 2`
* `1 ≤ ai, bi ≤ n`
* `1 ≤ t ≤ 50`
* `1 ≤ target ≤ n`

---

## 🎯 Intuition

This is a **probability propagation on a tree** problem.

Key observations:

* The frog always moves **level by level in time** → perfect for **BFS**
* At each second:

  * The probability at a node is **split equally** among its unvisited neighbors
  * If a node has **no unvisited neighbors**, the frog **stays there**
* Once a node is visited, it is never revisited

So we simulate the frog’s movement **second by second**, tracking probabilities.

---

## 🧠 BFS State Design

Each BFS state contains:

* `node` → current vertex
* `prob` → probability of being at this vertex at current time

We also track:

* `visited[node]` → to prevent revisits
* adjacency list for the tree

---

## 🚀 BFS Simulation Logic

1. Build adjacency list
2. Start BFS from `(node = 1, probability = 1.0)`
3. For each second:

   * For each node in the queue:

     * Count unvisited neighbors
     * If there are neighbors:

       * Split probability equally
       * Push neighbors into queue
     * Else:

       * Stay at same node
4. After `t` seconds:

   * If `target` is in queue → return its probability
   * Else → return `0.0`

---

## ✅ Java Solution (BFS)

```java
import java.util.*;
public class Solution{
    public class State {
        int node;
        double prob;
        public State(int n, double p) {
            node = n;
            prob = p;
        }
    }
    
    public double frogPosition(int n, int[][] edges, int t, int target) {
        List<List<Integer>> tree = new ArrayList<>();
        for(int i = 0; i <= n; i++) tree.add(i, new ArrayList<>());
        for(int[] e : edges) {
            tree.get(e[0]).add(e[1]);
            tree.get(e[1]).add(e[0]);
        }
        
        Queue<State> q = new ArrayDeque<>();
        boolean[] visited = new boolean[n + 1];
        q.offer(new State(1, 1.0));
        visited[1] = true;
        
        int timeLeft = t;
        while(!q.isEmpty() && timeLeft >= 0) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                State curr = q.poll();
                int node = curr.node;
                double p = curr.prob;
                
                int unvisitedCount = 0;
                for(int nbr : tree.get(node)) {
                    if(!visited[nbr]) unvisitedCount++;
                }
                
                if(node == target) {
                    if(unvisitedCount == 0 || timeLeft == 0) return p;
                    return 0.0;
                }
                
                if(unvisitedCount > 0) {
                    for(int nbr : tree.get(node)) {
                        double newP = p / unvisitedCount;
                        if(!visited[nbr]) {
                            visited[nbr] = true;
                            q.offer(new State(nbr, newP));
                        }
                    }
                }
            }
            timeLeft--;
        }
        
        return 0.0;
    }
}
```

#### ⏱ Complexity Analysis

* **Time Complexity:** `O(n + t)`
  * Each edge is processed once
  * BFS runs for `t ≤ 50` levels

* **Space Complexity:** `O(n)`
  * Graph + visited + BFS queue
