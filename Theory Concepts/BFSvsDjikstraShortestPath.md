Below is a **clear, decision-oriented explanation** of **when to use BFS vs Dijkstra’s shortest path**, with **practical signals** you can recognize during problem-solving or interviews.

---

## 🧭 Core Difference (One Line Summary)

> **BFS** is for **unweighted graphs** (or equal weights).
> **Dijkstra** is for **weighted graphs with non-negative weights**.

Everything else flows from this.

---

## 🟦 When to Use **BFS (Breadth-First Search)**

### ✅ Use BFS if **all edges have the same cost**

This includes:

* Unweighted graphs
* Graphs where every edge has cost = `1`

#### Why?

BFS explores nodes **level by level**, which guarantees the first time you reach a node is via the **shortest path** (in number of edges).

---

### 🔍 Typical BFS Problem Signals

If the problem says:

* “Minimum number of steps”
* “Minimum number of moves”
* “Shortest path in an unweighted graph”
* “Each move costs the same”
* “Transform one word to another (one change per step)”
* “Grid movement where each move costs 1”

➡️ **Use BFS**

---

### 🧠 BFS Examples

| Problem                                      | Why BFS                     |
| -------------------------------------------- | --------------------------- |
| Shortest path in a maze (up/down/left/right) | Each move costs 1           |
| Word Ladder                                  | Each transformation costs 1 |
| Minimum jumps                                | Uniform cost                |
| Rotten Oranges                               | Level-by-level spread       |
| Knight moves on chessboard                   | Each move = 1               |

---

### ⏱ Complexity (BFS)

* **Time:** `O(V + E)`
* **Space:** `O(V)`
* Uses a **Queue**

---

## 🟨 When to Use **Dijkstra’s Algorithm**

### ✅ Use Dijkstra if **edges have different weights**

Conditions:

* Edge weights are **non-negative**
* You want the **minimum total cost**, not minimum hops

#### Why?

BFS assumes equal cost edges — once weights vary, BFS can give **wrong answers**.

---

### 🔍 Typical Dijkstra Problem Signals

If the problem says:

* “Each edge has a weight”
* “Cost”, “time”, “distance”, “fare”, “energy”
* “Minimize total cost”
* “Weighted graph”

➡️ **Use Dijkstra**

---

### 🧠 Dijkstra Examples

| Problem                          | Why Dijkstra           |
| -------------------------------- | ---------------------- |
| Shortest distance between cities | Different road lengths |
| Flight with cheapest cost        | Varying prices         |
| Network delay time               | Different delays       |
| Minimum effort path              | Weighted transitions   |
| Graph with costs like 5, 10, 100 | Unequal weights        |

---

### ⏱ Complexity (Dijkstra)

* **Time:**

  * With Priority Queue: `O((V + E) log V)`
* **Space:** `O(V)`
* Uses a **Min-Heap / Priority Queue**

---

## 🚫 Important Limitation

❌ **Dijkstra does NOT work with negative weights**

If you see:

* Negative edge weights

➡️ Consider **Bellman–Ford**, not BFS or Dijkstra.

---

## 🧩 Side-by-Side Comparison

| Feature                   | BFS                   | Dijkstra               |
| ------------------------- | --------------------- | ---------------------- |
| Graph type                | Unweighted            | Weighted               |
| Edge weights              | All equal (usually 1) | Non-negative, variable |
| Guarantees shortest path? | Yes (by edges)        | Yes (by cost)          |
| Data structure            | Queue                 | Priority Queue         |
| Faster?                   | ✅ Faster              | ❌ Slower               |
| Handles negative weights  | ❌ No                  | ❌ No                   |

---

## 🧠 Interview Decision Shortcut (Very Important)

Ask yourself **ONE question**:

> ❓ *Do all moves/edges cost the same?*

* ✅ Yes → **BFS**
* ❌ No → **Dijkstra**

That’s it.

---

## 🔥 Tricky But Common Edge Case

### Graph with only `0` and `1` weights?

➡️ Use **0-1 BFS** (Deque), not standard BFS or Dijkstra.

---

## 🏁 Final Mental Model

* **BFS** → “Fewest steps”
* **Dijkstra** → “Cheapest path”
