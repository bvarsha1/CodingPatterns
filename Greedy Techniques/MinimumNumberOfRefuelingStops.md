## Minimum Number of Refueling Stops

#### **Statement**

You need to find the minimum number of refueling stops that a car needs to make to cover a distance, `target`. For simplicity, assume that the car has to travel from west to east in a straight line. There are various fuel stations on the way that are represented as a 2-D array of `stations`, i.e.,
`stations[i] = [d_i, f_i]`, where `d_i` is the distance (in miles) of the *i*th gas station from the starting position, and `f_i` is the amount of fuel (in liters) that it stores.

Initially, the car starts with `k` liters of fuel. The car consumes one liter of fuel for every mile traveled. Upon reaching a gas station, the car can stop and refuel using all the petrol stored at the station. If it cannot reach the target, the program returns `-1`.

> **Note:**
If the car reaches a station with `0` fuel left, it can refuel from that station, and all the fuel from that station can be transferred to the car. If the car reaches the target with `0` fuel left, it is still considered to have arrived.

---

#### ✔️ Constraints

* 1 ≤ `target`, `k` ≤ 10^9
* 0 ≤ `stations.length` ≤ 900
* 1 ≤ `d_i` < `d_{i+1}` < `target`
* 1 ≤ `f_i` < 10^9

---

## 🎯 Intuition

This problem is optimally solved using a **Greedy approach with a Max Heap**.

Key idea:

* Travel as far as possible with the current fuel
* Keep track of all fuel stations we have passed
* When we cannot move further, refuel from the **largest available fuel station** encountered so far

Approach:

* Use a max heap to store fuel amounts of reachable stations
* Move forward until fuel is insufficient
* Refuel from the station with the maximum fuel
* Repeat until the target is reached or no refueling option exists

This greedy choice ensures we minimize the number of refueling stops.

---

## ✅ Java Solution

```java
import java.util.*;

class MinimumRefuelStops {
  public static int minRefuelStops(int target, int startFuel, int[][] stations) {
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> Integer.compare(b , a));
    int fuel = startFuel;
    int stops = 0;
    int i = 0, n = stations.length;
    
    while(fuel < target) {
      // add all fuel stations to max heap in decreasing order of fuel stores
      // that we had option to refuel from on the way
      // until the fuel is exhausted 
      // basically, we add the stations that is within the distance that can be travelled in the car's fuel
      while(i < n && stations[i][0] <= fuel) {
        maxHeap.offer(stations[i][1]);
        i++;
      }
      
      // if no refueling options left, return -1 as its impossible to reach target
      // fuel will exhaust before we reach a station, or no stations left
      if(maxHeap.isEmpty()) return -1;
      
      // fuel is synonymous to the total distance travelled here, we keep on adding to it
      fuel += maxHeap.poll();
      stops++;
    }
    
    return stops;
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n)
* **Space Complexity:** O(n)