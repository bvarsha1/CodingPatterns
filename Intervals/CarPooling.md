## Car Pooling

#### **Statement**

You are given a car with a fixed number of seats, denoted by an integer capacity. The car only travels in one direction — eastward — and does not make any U-turns.

You are also provided with an array, trips, where each element trips[i] = [numPassengersᵢ, fromᵢ, toᵢ] represents a group of numPassengersᵢ that must be picked up at location fromᵢ and dropped off at location toᵢ. All locations are measured in kilometers east of the starting point.

Your task is to determine whether it is possible to complete all the trips without exceeding the car’s capacity at any point in time.

Return **TRUE** if all trips can be completed successfully, or **FALSE** otherwise.

---

#### ✔️ Constraints

* `1 ≤ trips.length ≤ 1000`
* `trips[i].length == 3`
* `1 ≤ numPassengersᵢ ≤ 100`
* `0 ≤ fromᵢ < toᵢ ≤ 1000`
* `1 ≤ capacity ≤ 10^5`

---

## 🎯 Intuition

This is a **difference array / sweep-line** problem:

* Every trip adds `numPassengersᵢ` at position `fromᵢ` and removes them at `toᵢ`.
* Instead of simulating every kilometer for every trip (inefficient), we:

  * Add passengers at pickup point.
  * Subtract passengers at drop-off point.
* After building this array, we accumulate values from left to right.
* If at any moment the running total exceeds `capacity`, the trips cannot be completed.

This guarantees efficient processing over a bounded range (0–1000).

---

## ✅ Java Solution

#### Approach

```java
import java.util.*;

public class Solution 
{
    public boolean carPooling(int[][] trips, int capacity) 
    {
        int[][] map = new int[trips.length * 2][2];
        
        int idx = 0;
        for(int[] trip : trips) {
            map[idx++] = new int[]{ trip[1], trip[0] };
            map[idx++] = new int[]{ trip[2], -trip[0] };
        }
        
        Arrays.sort(map, (int[] a, int[] b) -> {
            if(a[0] == b[0]) return Integer.compare(a[1], b[1]);
            else return Integer.compare(a[0], b[0]);
        });
        
        int curr = 0;
        for(int i = 0; i < map.length; i++) {
            curr += map[i][1];
            
            if(curr > capacity) return false;
        }
        
        return true;
    }
}
```
* **Time Complexity:** `O(nlogn)`

* **Space Complexity:** `O(n)`

#### Approach making best use of constraints

```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int[] diff = new int[1001];

        for(int[] t : trips) {
            int passengers = t[0];
            int from = t[1];
            int to = t[2];

            diff[from] += passengers;
            diff[to] -= passengers;
        }

        int curr = 0;
        for(int i = 0; i <= 1000; i++) {
            curr += diff[i];
            if(curr > capacity) return false;
        }

        return true;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:**
  `O(n + maxLocation)` → here = `O(1000 + trips.length)` ≈ `O(1000)`.

* **Space Complexity:**
  `O(1)` — fixed-size array of size 1001.
