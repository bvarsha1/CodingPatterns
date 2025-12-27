## Bus Routes

#### Statement

You are given an array, `routes`, representing bus routes where `routes[i]` is a bus route that the *i*th bus repeats forever. Every route contains one or more stations. You have also been given the source station, `src`, and a destination station, `dest`. Return the minimum number of buses someone must take to travel from `src` to `dest`, or return `-1` if there is no route.

---

#### ✔️ Constraints

* `1 ≤ routes.length ≤ 50`
* `1 ≤ routes[i].length ≤ 100`
* `0 ≤ routes[i][j] < 1000`
* `0 ≤ src, dest < 1000`

---

## 🎯 Intuition

This is a **graph shortest path** problem, but with a twist:

* **Stations are nodes**
* **Buses are edges connecting multiple stations**
* Each bus taken counts as **1 step**, regardless of how many stations it covers

Key ideas:

* Build a mapping from **station → buses that stop there**
* Perform **BFS** starting from `src`
* Each BFS level represents **taking one more bus**
* Once a bus is used, mark it as visited to avoid reusing it
* From a bus, we can reach **all its stations**

The first time we reach `dest`, we have taken the **minimum number of buses**.

---

## ✅ Java Solution

```java
import java.util.*;

class MinimumBuses {
  public static int minimumBuses(int[][] busRoutes, int src, int dest) {
    HashMap<Integer, List<Integer>> stationBusMap = new HashMap<>();
    
    for(int i = 0; i < busRoutes.length; i++) {
      for(int s : busRoutes[i]) {
        stationBusMap.putIfAbsent(s, new ArrayList<>());
        stationBusMap.get(s).add(i);
      }
    }
    
    // maintain a queue of stations
    Queue<Integer> q = new ArrayDeque<>();
    boolean[] busTaken = new boolean[busRoutes.length];
    q.offer(src);
    int buses = 0;
    
    while(!q.isEmpty()) {
      int size = q.size();
      for(int i = 0; i < size; i++) {
        int curr = q.poll();
        if(curr == dest) return buses;
        
        // check all buses available from the station
        for(int bus : stationBusMap.getOrDefault(curr, new ArrayList<>())) {
          if(!busTaken[bus]) {
            busTaken[bus] = true;
            for(int s : busRoutes[bus]) {
              q.offer(s);
            }
          }
        }
      }
      buses++;
    }
    return -1;
  }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(total stations across all routes)`
* **Space Complexity:** `O(total stations + number of buses)`
