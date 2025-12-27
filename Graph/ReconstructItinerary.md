## Reconstruct Itinerary

#### Statement

Given a list of airline tickets where `tickets[i] = [fromi, toi]` represent a departure airport and an arrival airport of a single flight, reconstruct the itinerary in the correct order and return it.

The person who owns these tickets always starts their journey from `"JFK"`. Therefore, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should prioritize the one with the smallest lexical order when considering a single string.

For example, the itinerary `["JFK", "EDU"]` has a smaller lexical order than `["JFK", "EDX"]`.

**Note:** You may assume all tickets form at least one valid itinerary. You must use all the tickets exactly once.

---

#### ✔️ Constraints

* `1 ≤ tickets.length ≤ 300`
* `tickets[i].length = 2`
* `fromi.length = 3`
* `toi.length = 3`
* `fromi != toi`
* `fromi` and `toi` consist of uppercase English letters.

---

## 🎯 Intuition

This problem is a classic **graph + Eulerian path** problem.

Key observations:

* Each ticket is a **directed edge** from `from → to`
* We must:

  * Start from `"JFK"`
  * Use **every edge exactly once**
  * Choose the **lexicographically smallest** valid path if multiple exist

This directly maps to **Hierholzer’s Algorithm** for finding an Eulerian path in a directed graph.

### Strategy

1. Build a graph:
   * `from → list of destinations`

2. Sort each destination list lexicographically
   * Ensures smallest lexical choice is used first

3. Perform **DFS**:
   * Always take the smallest available destination
   * Remove edges as they are used

4. Add airports to the answer **after exploring all outgoing flights**
   * This produces the itinerary in **reverse**
   
5. Reverse the result at the end

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static List<String> findItinerary(List<List<String>> tickets) {
        List<String> itinerary = new ArrayList<>();

        // Build graph: src -> sorted list of destinations
        Map<String, LinkedList<String>> flights = new HashMap<>();
        for (List<String> ticket : tickets) {
            flights.putIfAbsent(ticket.get(0), new LinkedList<>());
            flights.get(ticket.get(0)).add(ticket.get(1));
        }

        // Sort destinations lexicographically
        for (LinkedList<String> dests : flights.values()) {
            Collections.sort(dests);
        }

        // DFS from JFK
        dfs("JFK", flights, itinerary);

        // Reverse because nodes are added post-order
        Collections.reverse(itinerary);
        return itinerary;
    }

    private static void dfs(
            String src,
            Map<String, LinkedList<String>> flights,
            List<String> itinerary
    ) {
        LinkedList<String> destinations =
                flights.getOrDefault(src, new LinkedList<>());

        while (!destinations.isEmpty()) {
            String next = destinations.pollFirst();
            dfs(next, flights, itinerary);
        }

        itinerary.add(src);
    }
}
```

## ⏱ Complexity

* **Time Complexity:** `O(E log E)`
  * Sorting destinations dominates
* **Space Complexity:** `O(E + V)`
  * Graph + recursion stack
