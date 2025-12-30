## Find Minimum Diameter After Merging Two Trees

#### Statement

You are given two undirected trees: one with `n` nodes labeled from `0` to `n−1`, and another with `m` nodes labeled from `0` to `m−1`. Their structures are defined by two 2D integer arrays—`edges1` of length `n−1` for the first tree, and `edges2` of length `m−1` for the second. Each element `edges1[i] = [aᵢ, bᵢ]` represents an edge between nodes `aᵢ` and `bᵢ` in the first tree, and similarly, `edges2[i] = [uᵢ, vᵢ]` represents an edge in the second tree.

Your task is to connect any node from the first tree to any node from the second tree using a single edge. Return the smallest possible diameter of the resulting combined tree.

> **Note:** The **diameter** of a tree is the length of the longest path between any two nodes in it.

---

#### ✔️ Constraints

* `1 <= n, m <= 10^5`
* `edges1.length == n - 1`
* `edges2.length == m - 1`
* `edges1[i].length == edges2[i].length == 2`
* `0 <= ai, bi < n`
* `0 <= ui, vi < m`
* `edges1` and `edges2` always represent valid trees

---

## 🎯 Intuition

This is a **tree diameter optimization** problem.

### Step 1: Understand what changes when we merge two trees

When you connect **one node from Tree-1** to **one node from Tree-2** with a single edge:

* Any path **entirely inside Tree-1** stays ≤ `diameter1`
* Any path **entirely inside Tree-2** stays ≤ `diameter2`
* New paths can go:

  * from a node in Tree-1 → connecting edge → a node in Tree-2

So the final diameter is:

```
max(
  diameter1,
  diameter2,
  longest path that goes across both trees
)
```

---

### Step 2: What is the longest cross-tree path?

To minimize the diameter, we should connect the trees at their **centers**.

Key property of trees:

* If a tree has diameter `D`
* The **minimum possible max distance from a chosen root to all nodes** is:

  ```
  ceil(D / 2) = (D + 1) / 2
  ```

So if we connect:

* center of Tree-1
* center of Tree-2

Then the longest cross-tree path is:

```
1 (new edge)
+ ceil(diameter1 / 2)
+ ceil(diameter2 / 2)
```

Which is computed as:

```
1 + (diameter1 + 1) / 2 + (diameter2 + 1) / 2
```

---

### Step 3: Final Answer

```
max(
  diameter1,
  diameter2,
  1 + ceil(diameter1 / 2) + ceil(diameter2 / 2)
)
```

---

## 🧠 How do we compute the diameter of a tree?

Using **2 BFS passes**:

1. BFS from any node (say `0`) → find the **farthest node**
2. BFS again from that farthest node → the maximum distance found is the **diameter**

This works because trees have **unique simple paths**.

---

## ✅ Java Solution (BFS + Tree Diameter Property)

```java
import java.util.*;

class Solution {
    
    public int minimumDiameterAfterMerge(int[][] edges1, int[][] edges2) {
        List<List<Integer>> tree1 = new ArrayList<>();
        List<List<Integer>> tree2 = new ArrayList<>();
        
        // build adjacency lists
        for (int i = 0; i <= edges1.length; i++) {
            tree1.add(new ArrayList<>());
        }
        for (int i = 0; i <= edges2.length; i++) {
            tree2.add(new ArrayList<>());
        }
        
        for (int[] e : edges1) {
            tree1.get(e[0]).add(e[1]);
            tree1.get(e[1]).add(e[0]);
        }
        
        for (int[] e : edges2) {
            tree2.get(e[0]).add(e[1]);
            tree2.get(e[1]).add(e[0]);
        }
        
        // diameter of tree 1
        int farthest1 = farthestBFS(0, tree1)[0];
        int diameter1 = farthestBFS(farthest1, tree1)[1];
        
        // diameter of tree 2
        int farthest2 = farthestBFS(0, tree2)[0];
        int diameter2 = farthestBFS(farthest2, tree2)[1];
        
        // best possible diameter after merge
        int merged = 1 + (diameter1 + 1) / 2 + (diameter2 + 1) / 2;
        
        return Math.max(merged, Math.max(diameter1, diameter2));
    }
    
    // BFS that returns {farthestNode, distance}
    public int[] farthestBFS(int start, List<List<Integer>> tree) {
        int n = tree.size();
        boolean[] visited = new boolean[n];
        Queue<Integer> q = new ArrayDeque<>();
        
        q.offer(start);
        visited[start] = true;
        
        int distance = 0;
        int farthest = start;
        
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                int curr = q.poll();
                farthest = curr;
                for (int nei : tree.get(curr)) {
                    if (!visited[nei]) {
                        visited[nei] = true;
                        q.offer(nei);
                    }
                }
            }
            distance++;
        }
        
        return new int[]{farthest, distance - 1};
    }
}
```

#### ⏱ Complexity Analysis

* **Time Complexity:** `O(n + m)`
  * Each tree is traversed twice using BFS

* **Space Complexity:** `O(n + m)`
  * Adjacency lists, visited arrays, BFS queues
