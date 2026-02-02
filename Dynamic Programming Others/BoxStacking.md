## **Box Stacking Problem**

#### Statement

Each box is defined by its three dimensions: `[height, weight, and depth]`. We can only place a box on top of another if it has strictly smaller dimensions in height, weight, and depth.

Determine the maximum `height` that can be achieved by stacking a selection of boxes.

---

#### ✔️ Constraints

* Each box has exactly three dimensions: height, weight, and depth
* A box can be stacked on another box only if **all three dimensions** are strictly smaller
* Any box may be used at most once
* The order of stacking matters
* The box cannot be rotated

---

## 🎯 Intuition

We want to **maximize the total height** of a stack of boxes under strict ordering rules.

Key observations:

* This is **not greedy** — choosing the tallest box first may block future stacks
* A box can only go **on top of larger boxes**
* This resembles:

  * **Longest Increasing Subsequence (LIS)**
  * with **3 dimensions**
  * and **height accumulation**

So the problem becomes:

> What is the maximum height stack ending at each box?

### 🧠 Approach (Dynamic Programming)

1. Treat each box as a potential **base**
2. Sort boxes based on dimensions (commonly height, then weight, then depth)
3. Define DP:

```
dp[i] = maximum stack height with box i at the bottom
```

4. Transition:

```
dp[i] = height[i] + max(dp[j])
where box j can be placed on box i
```

Meaning:

* box j must have **strictly smaller height, weight, and depth** than box i

5. The answer is:

```
max(dp[i]) for all i
```

---

## ✅ Java Solution

```java
import java.util.*;

class Box {
    int h, w, d;
    Box(int h, int w, int d) {
        this.h = h;
        this.w = w;
        this.d = d;
    }
}

public class BoxStacking {

    public static int maxStackHeight(Box[] boxes) {
        int n = boxes.length;

        Arrays.sort(boxes, (a, b) -> {a.h - b.h});

        int[] dp = new int[n];
        int maxH = 0;

        for(int i = 0; i < n; i++) {
            dp[i] = boxes[i].h;
            for(int j = 0; j < i; j++) {
                if(
                    boxes[j].w < boxes[i].w && 
                    boxes[j].d < boxes[i].d && 
                    boxes[j].h < boxes[i].h
                ) {
                    dp[i] = Math.max(dp[i], dp[j] + boxes[i].h);
                }
            }
            maxH = Math.max(maxH, dp[i]);
        }

        return maxH;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n²)`
* **Space Complexity:** `O(n)`
