## Open the Lock

#### Statement

You are given a lock with **4 circular wheels**, each containing digits `'0'` through `'9'`.

* The wheels can rotate freely and wrap around cyclically; turning `'9'` forward leads to `'0'`, and turning `'0'` backward leads to `'9'`.

* Each move consists of rotating **one wheel by one position** (either forward or backward).

The lock starts from the initial state `"0000"`.

You are also given a list of **deadends**, where each string represents a lock position that is blocked; if the lock reaches any of these positions, it becomes stuck and can no longer be turned.

Your goal is to reach a given **target** combination by performing the **minimum number of moves**, starting from `"0000"` and avoiding all dead ends.

If it is impossible to reach the target, return `-1`.

---

#### ✔️ Constraints

* `1 ≤ deadends.length ≤ 500`
* `deadends[i].length == 4`
* `target.length == 4`
* `target` will not be in `deadends`
* `target` and `deadends[i]` consist of digits only

---

## 🎯 Intuition

This is a **shortest path problem on an implicit graph**.

### How to model the problem

* Each lock combination (e.g. `"0193"`) is a **node**
* From any node, you can generate **up to 8 neighbors**:

  * For each of the 4 wheels:

    * Rotate forward
    * Rotate backward
* Each move has **equal cost = 1**

So the task becomes:

> Find the **shortest number of moves** from `"0000"` to `target`, without visiting any deadend state.

This is a classic **Breadth First Search (BFS)** problem.

---

## 🧠 BFS Strategy

Why BFS?

* BFS guarantees the **shortest path** in an unweighted graph
* The first time we reach `target`, we have the minimum moves

### Key steps

1. Store all deadends in a `HashSet` for O(1) lookup
2. Use another `HashSet` to track visited states
3. Start BFS from `"0000"`
4. For each state:

   * Generate all valid next states
   * Skip visited or deadend states
5. Count BFS levels → number of moves

---

## ✅ Java Solution (BFS)

```java
import java.util.*;

public class Solution {
    public static int openLock(String[] deadends, String target) {
        Set<String> dead = new HashSet<>(Arrays.asList(deadends));
        Set<String> visited = new HashSet<>();
        
        // initial state blocked
        if (dead.contains("0000")) return -1;
        
        Queue<String> q = new ArrayDeque<>();
        q.offer("0000");
        visited.add("0000");
        
        int moves = 0;
        
        while (!q.isEmpty()) {
            int size = q.size();
            
            for (int i = 0; i < size; i++) {
                String curr = q.poll();
                
                // reached target
                if (curr.equals(target)) return moves;
                
                // generate next states
                for (int w = 0; w < 4; w++) {
                    char[] arr = curr.toCharArray();
                    
                    // rotate forward
                    arr[w] = arr[w] == '9' ? '0' : (char)(arr[w] + 1);
                    String next1 = new String(arr);
                    
                    // rotate backward
                    arr[w] = curr.charAt(w) == '0' ? '9' : (char)(curr.charAt(w) - 1);
                    String next2 = new String(arr);
                    
                    if (!dead.contains(next1) && visited.add(next1)) {
                        q.offer(next1);
                    }
                    
                    if (!dead.contains(next2) && visited.add(next2)) {
                        q.offer(next2);
                    }
                }
            }
            
            moves++;
        }
        
        return -1;
    }
}
```

#### ⏱ Complexity Analysis

* **Time Complexity:** `O(10^4)`
  * Total possible lock states = `10^4`
  * Each state generates 8 neighbors

* **Space Complexity:** `O(10^4)`
  * Queue + visited set