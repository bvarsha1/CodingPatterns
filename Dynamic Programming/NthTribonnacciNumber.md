## N-th Tribonacci Number

#### **Statement**

Given a number `n`, calculate the corresponding Tribonacci number. The Tribonacci sequence Tₙ is defined as:

T₀ = 0, T₁ = 1, T₂ = 1, and
Tₙ₊₃ = Tₙ + Tₙ₊₁ + Tₙ₊₂, for n ≥ 0

The input number, `n`, is a non-negative integer.

---

#### ✔️ Constraints

* 0 ≤ `n` ≤ 37
* The answer is guaranteed to fit within a 32-bit integer, i.e., answer ≤ 2³¹ − 1

---

## 🎯 Intuition

This is similar to Fibonacci but each value depends on the **previous three** terms:

```
T[n] = T[n-1] + T[n-2] + T[n-3]
```

We can compute it iteratively starting from the base cases:

* T0 = 0
* T1 = 1
* T2 = 1

Just keep updating three rolling variables until we reach `n`.

---

## ✅ Java Solution

```java
class Solution {
    public int tribonacci(int n) {
        if (n == 0) return 0;
        if (n == 1 || n == 2) return 1;

        int a = 0, b = 1, c = 1;
        int d = 0;

        for (int i = 3; i <= n; i++) {
            d = a + b + c;
            a = b;
            b = c;
            c = d;
        }

        return c;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
