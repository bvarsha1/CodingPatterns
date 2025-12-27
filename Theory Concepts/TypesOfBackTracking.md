## 🧠 The Two Types of Backtracking

### 1️⃣ **Decision Backtracking (Exploratory Backtracking)**

👉 *“Try a choice, if it fails, undo it and try another.”*

#### Typical use cases

* Permutations / combinations
* N-Queens
* Sudoku
* Subsets
* Word search

#### How it works

1. Make a choice
2. Go deeper
3. If it leads to an invalid state:

   * **Undo the choice**
   * Try the next option

#### Key characteristics

* Explicit **undo**
* Explicit **failure detection**
* Tree of possibilities

#### Example (intuition)

```text
Choose A
  Choose B
    ❌ invalid → undo B
  Choose C
    ✅ valid
```

This is the **classic backtracking** most people learn first.

---

### 2️⃣ **Structural Backtracking (Post-order / Stack Backtracking)**

👉 *“Go as far as possible, then add result while returning.”*

This is the type used in **Reconstruct Itinerary**.

#### Typical use cases

* Eulerian Path / Circuit
* Topological ordering variants
* Post-order DFS constructions
* Graph problems where **edges must be used exactly once**

#### How it works

1. Keep moving forward while possible
2. **When stuck**, return to the previous node
3. Add nodes **while unwinding the recursion stack**

#### Key characteristics

* No “invalid” state
* No explicit undo
* Backtracking happens via **call stack**
* Result is built **after exploring everything**

---

## 🎯 Key Difference (Most Important Insight)

| Decision Backtracking | Structural Backtracking  |
| --------------------- | ------------------------ |
| Tries alternatives    | Consumes structure       |
| Backtracks on failure | Backtracks on exhaustion |
| Undo choices          | Finish paths             |
| Pre-order thinking    | **Post-order thinking**  |

---

## 🛫 Applying This to **Reconstruct Itinerary**

### Problem requirements

* Use **every ticket exactly once**
* Start from `"JFK"`
* Choose **lexicographically smallest** option
* Always guaranteed a solution

This means:
👉 We are **not choosing between valid/invalid paths**
👉 We are **consuming all edges exactly once**

That is **Eulerian Path**.

---

## 🔁 Where Backtracking Actually Happens

Let’s revisit your example:

```
JFK → ATL
JFK → SFO
SFO → JFK
```

### Adjacency (sorted)

```
JFK: [ATL, SFO]
SFO: [JFK]
```

---

### DFS execution

```text
dfs("JFK")
  → take ATL
    dfs("ATL")
      no outgoing edges
      add ATL to result
  → take SFO
    dfs("SFO")
      → take JFK
        dfs("JFK")
          no outgoing edges
          add JFK
      add SFO
  add JFK
```

---

### Result build order (post-order)

```
[ATL, JFK, SFO, JFK]
```

Reverse it:

```
[JFK, SFO, JFK, ATL]
```

✔ All tickets used
✔ Correct order
✔ Lexicographically smallest

---

### 🚨 Important realization

You **did backtrack**, but:

* Not because ATL was “wrong”
* Because ATL was **finished**
* You returned because there were **no more edges**

That’s **structural backtracking**.

---

## 🧩 Mental Model That Helps

### Decision Backtracking

> “Is this choice correct?”

### Structural Backtracking

> “Am I done with this node?”

In Reconstruct Itinerary:

* You **never reject a path**
* You just **finish it**

---

## 🧠 One-Line Summary (Memorize This)

> **Decision backtracking explores choices.
> Structural backtracking consumes structure.**

---

## 🏁 Final Takeaway

When you see:

* “Use every edge exactly once”
* “Start from a fixed node”
* “Graph traversal + ordering”

➡️ Think **Eulerian Path**
➡️ Think **post-order DFS**
➡️ Think **structural backtracking**