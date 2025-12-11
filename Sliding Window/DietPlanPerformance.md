## **Diet Plan Performance**

#### **Statement**

A dieter consumes `calories[i]` calories on the *i-th* day.

Given an integer **k**, the dieter reviews their calorie intake for **every sequence of k consecutive days** (from `calories[i]` to `calories[i + k − 1]` for all `0 ≤ i ≤ n − k`).
For each sequence, they compute **T = total calories over those k days**:

* If **T < lower**, the dieter performs poorly → **loses 1 point**
* If **T > upper**, the dieter performs better → **gains 1 point**
* If **lower ≤ T ≤ upper**, performance is normal → **0 points change**

The dieter starts with **0 points**.
Return the **final total points** after evaluating all sequences.
The total points may be **negative**.

---

#### ✔️ **Constraints**

* `1 ≤ k ≤ calories.length ≤ 10^5`
* `0 ≤ calories[i] ≤ 20000`
* `0 ≤ lower ≤ upper`

---

## 🎯 **Intuition**

We need to repeatedly compute the **sum of every window of size k**.
A brute force approach would compute each window independently → **O(n · k)**, far too slow for `10^5` elements.

Instead, we use a **sliding window**:

### 🔹 Key Idea

1. Compute the sum of the **first k days**.
2. For each next day:

   * Add the incoming day's calories
   * Subtract the outgoing day's calories
     → This updates the window sum in **O(1)** time.
3. For each window sum:

   * Compare with `lower` and `upper`
   * Update points accordingly

This makes the entire process **linear**, i.e., **O(n)**.

---

## ✅ **Java Solution**

```java
import java.util.List;

public class Solution {

    public static int dietPlanPerformance(List<Integer> calories, int k, int lower, int upper) {
      int sum = 0;

      // Initial window sum
      for (int i = 0; i < k; i++)
        sum += calories.get(i);
      
      int points = 0;

      // Slide the window across the list
      for (int i = k; i <= calories.size(); i++) {

        // Evaluate this window
        if (sum < lower) points--;
        else if (sum > upper) points++;

        // Slide window forward
        if (i < calories.size())
          sum += calories.get(i) - calories.get(i - k);
      }
      
      return points;
    }
}
```

#### ⏱️ **Complexity**

* **Time Complexity:** O(n)
  Each index enters and leaves the window once.

* **Space Complexity:** O(1)
  Only variables for sum and points are used.
