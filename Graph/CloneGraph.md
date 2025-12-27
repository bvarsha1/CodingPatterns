## Clone Graph

#### Statement

You are given a reference to a single node in an undirected, connected graph. Your task is to create a deep copy of the graph starting from the given node. A deep copy means creating a new instance of every node in the graph with the same data and edges as the original graph, such that changes in the copied graph do not affect the original graph.

Each node in the graph contains two properties:

* `data`: The value of the node, which is the same as its index in the adjacency list.
* `neighbors`: A list of connected nodes, representing all the nodes directly linked to this node.

However, in the test cases, a graph is represented as an adjacency list to understand node relationships, where each index in the list represents a node (using 1-based indexing). For example, for

```
[[2,3],[1,3],[1,2]]
```

there are three nodes in the graph:

* `1st` node (`data = 1`): Neighbors are `2nd` node (`data = 2`) and `3rd` node (`data = 3`)
* `2nd` node (`data = 2`): Neighbors are `1st` node (`data = 1`) and `3rd` node (`data = 3`)
* `3rd` node (`data = 3`): Neighbors are `1st` node (`data = 1`) and `2nd` node (`data = 2`)

The adjacency list will be converted to a graph at the backend and the first node of the graph will be passed to your code.

---

#### ✔️ Constraints

* `0 ≤ Number of nodes ≤ 100`
* `1 ≤ Node.data ≤ 100`
* `Node.data` is unique for each node.

---

## 🎯 Intuition

To clone the graph, we must ensure:

* **Each original node is copied exactly once**
* **All neighbor relationships are preserved**
* **Cycles are handled safely** (to avoid infinite loops)

Key idea:

* Use a **HashMap** to store the mapping from original nodes to their cloned nodes
* Traverse the graph using **DFS or BFS**
* Whenever we encounter a node:

  * If it hasn’t been cloned yet → create a clone
  * Add cloned neighbors by recursively cloning adjacent nodes

The map ensures:

* No duplicate node creation
* Correct handling of cycles and shared neighbors

---

## ✅ Java Solution (DFS)

```java
/*

import java.util.*;

class Node {
    int data;
    List<Node> neighbors;

    public Node(int data) {
        this.data = data;
        this.neighbors = new ArrayList<Node>();
    }
}

*/

import java.util.*;

public class CloneGraph{
    public static Node clone(Node root) {
        Map<Node, Node> cloned = new HashMap<>();
        Queue<Node> q = new ArrayDeque<>();
        q.offer(root);
        cloned.put(root, new Node(root.data));
        
        while(!q.isEmpty()) {
            Node curr = q.poll();
            Node clone = cloned.get(curr);
            
            for(Node nbr : curr.neighbors) {
                if(!cloned.containsKey(nbr)) {
                    Node clonedNbr = new Node(nbr.data);
                    cloned.put(nbr, clonedNbr);
                    q.offer(nbr);
                }
                clone.neighbors.add(cloned.get(nbr));
            }
        }
        return cloned.get(root);
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** O(V + E), where `V` is the number of nodes and `E` is the number of edges
* **Space Complexity:** O(V) for the hashmap and recursion stack