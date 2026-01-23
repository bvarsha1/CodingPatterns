## Boats to Save People

#### **Statement**

A big ship with numerous passengers is sinking, and there is a need to evacuate these people with the minimum number of life-saving boats. Each boat can carry, at most, two persons however, the weight of the people cannot exceed the carrying weight limit of the boat.

We are given an array, `people`, where `people[i]` is the weight of the *i*th person, and an infinite number of boats, where each boat can carry a maximum weight, `limit`. Each boat carries, at most, two people at the same time. This is provided that the sum of the weight of these people is under or equal to the weight limit.

You need to return the minimum number of boats to carry all persons in the array.

---

#### ✔️ Constraints

* 1 ≤ `people.length` ≤ 5 × 10^3
* 1 ≤ `people[i]` ≤ `limit` ≤ 3 × 10^3

---

## 🎯 Intuition

This problem is best solved using a **Greedy + Two Pointers** approach.

To minimize the number of boats:

* Sort the people by weight
* Try to pair the **lightest** person with the **heaviest** person
* If their combined weight is within the limit, put them in one boat
* Otherwise, the heaviest person must go alone

This greedy strategy works because pairing the heaviest person with the lightest possible person gives the **best chance to save a boat**.

---

## ✅ Java Solution

```java
import java.util.Arrays;

public class Solution {
    public static int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);

        int left = 0;
        int right = people.length - 1;
        int boats = 0;

        while (left <= right) {
            if (people[left] + people[right] <= limit) {
                left++;
            }
            right--;
            boats++;
        }

        return boats;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n) — due to sorting
* **Space Complexity:** O(1) — ignoring sort space