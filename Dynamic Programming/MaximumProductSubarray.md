## Maximum Product Subarray

#### **Statement**

Given an integer array, `nums`, find a subarray that has the largest product, and return the product.

---

#### ✔️ Constraints

* `1 ≤ nums.length ≤ 10^3`
* `−10 ≤ nums[i] ≤ 10`
* The product of any prefix or suffix of nums is guaranteed to fit in a 32−bit integer.

---

## 🎯 Intuition

This problem is tricky because the product can flip signs:

* Multiplying by a **negative** turns max ↔ min
* Multiplying by **zero** resets the product

Therefore, at each index, we track two values:

* `maxProd` → maximum product ending at current index
* `minProd` → minimum product ending at current index (could become max if next number is negative)

Transition:

```
tempMax = max(nums[i], nums[i] * maxProd, nums[i] * minProd)
tempMin = min(nums[i], nums[i] * maxProd, nums[i] * minProd)
```

Global result is the maximum over all `maxProd`.

---

## ✅ Java Solution

```java
import java.util.*;

public class MaxProduct{
  public static int maxProduct(int [] nums) {
    int min = nums[0], max = nums[0], ans = nums[0];
    
    for(int i = 1; i < nums.length; i++) {
      int n = nums[i];
      int temp = max * n;
      max = Math.max(n, Math.max(max * n, min * n));
      min = Math.min(n, Math.min(temp, min * n));
      
      ans = Math.max(ans, max);
    }
    
    return ans;
  }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)