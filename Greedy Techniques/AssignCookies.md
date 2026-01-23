## Assign Cookies

#### **Statement**

You are given two integer arrays:

`greedFactors`: This array represents the minimum cookie size required for each child to be content. Specifically, `greedFactors[i]` denotes the minimum cookie size that child *i* will accept to be satisfied.

`cookieSizes`: This array represents the sizes of the available cookies. For example, `cookieSizes[j]` denotes the size of cookie *j*.

Your objective is to distribute the cookies in a way that maximizes the number of content children. A child *i* is content if they receive a cookie that is at least as large as their greed factor, i.e.,
`cookieSizes[j] >= greedFactors[i]`.

Each child can be assigned at most one cookie, which can only be given to one child.

Write an algorithm to maximize the number of children who can be content by assigning available cookies appropriately.

---

#### ✔️ Constraints

* 1 ≤ `greedFactors.length` ≤ 1000
* 0 ≤ `cookieSizes.length` ≤ 1000
* 1 ≤ `greedFactors[i]`, `cookieSizes[j]` ≤ 10^5

---

## 🎯 Intuition

This problem is a classic **Greedy + Two Pointers** problem.

Key idea:

* To satisfy as many children as possible, assign the **smallest available cookie** that can satisfy the **least greedy child**
* Wasting a large cookie on a child with small greed reduces future possibilities

Approach:

1. Sort both `greedFactors` and `cookieSizes`
2. Use two pointers:

   * One for children (least greedy first)
   * One for cookies (smallest size first)
3. If the current cookie can satisfy the current child:

   * Assign it and move both pointers
4. Otherwise:

   * Try a larger cookie

This greedy strategy ensures the maximum number of content children.

---

## ✅ Java Solution

```java
import java.util.Arrays;

public class Solution {
    public static int findContentChildren(int[] greedFactors, int[] cookieSizes) {
        Arrays.sort(greedFactors);
        Arrays.sort(cookieSizes);

        int child = 0;
        int cookie = 0;
        int content = 0;

        while (child < greedFactors.length && cookie < cookieSizes.length) {
            if (cookieSizes[cookie] >= greedFactors[child]) {
                content++;
                child++;
                cookie++;
            } else {
                cookie++;
            }
        }

        return content;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n) — due to sorting
* **Space Complexity:** O(1) — ignoring sort space