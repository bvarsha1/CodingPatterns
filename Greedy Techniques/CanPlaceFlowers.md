## Can Place Flowers

#### **Statement**

Given an integer list `flowerbed`, each element is either `0` (indicating an empty plot) or `1` (indicating a planted plot), and an integer `n`. Determine if `n` new flowers can be planted without violating the rule that no two flowers can be planted in adjacent plots. Return `TRUE` if it’s possible to plant all `n` flowers. Otherwise, return `FALSE`.

---

#### ✔️ Constraints

* 1 ≤ `flowerbed.length` ≤ 10^3
* 0 ≤ `n` ≤ `flowerbed.length`
* `flowerbed[i]` is `0` or `1`
* There are no two adjacent flowers in the flowerbed

---

## 🎯 Intuition

A greedy approach works well here:

* We iterate from left to right
* At each position `i`, we check if:

  1. `flowerbed[i] == 0`
  2. Left neighbor is empty or doesn't exist
  3. Right neighbor is empty or doesn't exist

If all conditions hold, we plant a flower there (set it to `1`) and decrement `n`.

Stop early if `n` becomes `0`.

---

## ✅ Java Solution

```java
class Solution {
  public boolean canPlaceFlowers(int[] flowerbed, int n) {
    int len = flowerbed.length;
    for(int i = 0; i < flowerbed.length && n > 0; i++) {
      if(flowerbed[i] == 0) {
        boolean left = (i == 0 || flowerbed[i - 1] == 0);
        boolean right = (i == len - 1 || flowerbed[i + 1] == 0);
        
        if(right && left) {
          flowerbed[i] = 1;
          n--;
        }
      }
    }
    
    return n == 0;
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)