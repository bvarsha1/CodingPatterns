## Candy

#### **Statement**

You are given an array, `ratings`, where `ratings[i]` represents the rating of the i-th child standing in a line. Your task is to distribute candies to each child based on the following rules:

1. Every child must receive at least one candy.

2. Children with a higher rating get more candies than their neighbors.

Determine the minimum total number of candies you must distribute to satisfy the above conditions.

---

#### ✔️ Constraints

* 1 ≤ `ratings.length` ≤ 1000
* 0 ≤ `ratings[i]` ≤ 1000

---

## 🎯 Intuition

This is a classic **two-pass greedy** problem.

Key idea:

* First, ensure the condition for children having higher rating than their **left neighbor**
* Then ensure for their **right neighbor**

Approach:

1. Create an array `candies` initialized with `1` for all children
2. Left-to-right pass:

   * If `ratings[i] > ratings[i-1]`, assign `candies[i] = candies[i-1] + 1`
3. Right-to-left pass:

   * If `ratings[i] > ratings[i+1]`, assign `candies[i] = max(candies[i], candies[i+1] + 1)`
4. Sum all candies

This guarantees minimal candy distribution while satisfying both neighbor constraints.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution{
   public static int candy(int[] ratings) {
      int n = ratings.length;
      int[] candy = new int[n];
      Arrays.fill(candy, 1);
      
      for(int i = 1; i < n; i++) {
         if(ratings[i] > ratings[i - 1]) {
            candy[i] = candy[i - 1] + 1;
         }
      }
      
      for(int i = n - 2; i >= 0; i--) {
         if(ratings[i] > ratings[i + 1] && candy[i] <= candy[i + 1]) {
            candy[i] = candy[i + 1] + 1;
         }
      }
      
      int totalCandies = 0;
      for(int i = 0; i < n; i++) {
         totalCandies += candy[i];
      }
      
      return totalCandies;
   }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n) — two passes + sum
* **Space Complexity:** O(n) — for the candies array