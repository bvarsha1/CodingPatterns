## Find Center of Star Graph

#### Statement

Given an array `edges` where each element `edges[i] = [ui, vi]` represents an edge between nodes `ui` and `vi` in an undirected star graph, find the central node of this star graph.

> **Note:** A star graph is a graph where one central node is connected to every other node. This implies that a star graph with `n` nodes has exactly `n - 1` edges.

---

#### ✔️ Constraints

* `3 ≤ n ≤ 10³`
* `edges.length = n - 1`
* `edges[i].length = 2`
* `1 ≤ ui, vi ≤ n`
* `ui ≠ vi`
* The given edges represent a valid star graph.

---

## 🎯 Intuition

In a **star graph**:

* There is **exactly one central node**
* That central node appears in **every edge**
* All other nodes appear only once

**Key observation:**

👉 Since every edge contains the center, **the center must be the common node between the first two edges**.

So, we don’t need to build a graph or count degrees for all nodes.

Steps:

1. Look at the first two edges:
   `edges[0] = [a, b]`
   `edges[1] = [c, d]`
2. The node that appears in **both edges** is the center.

This works because the input is guaranteed to be a valid star graph.

---

## ✅ Java Solution

```java
public class Solution {
    public static int findCenter(int[][] edges) {
        int a = edges[0][0];
        int b = edges[0][1];
        int c = edges[1][0];
        int d = edges[1][1];

        // The common node between the first two edges is the center
        if (a == c || a == d) {
            return a;
        }
        return b;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(1)`
* **Space Complexity:** `O(1)`