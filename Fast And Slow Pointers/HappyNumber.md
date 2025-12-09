## Happy Number

#### Statement

Write an algorithm to determine if a number `n` is a **happy number**.

We use the following process:

1️⃣ Start with number `n`
2️⃣ Replace the number with **the sum of the squares of its digits**
3️⃣ Repeat until:

* The number becomes **1** → `n` is a **happy number** ✔️
* The number **enters a cycle** → `n` is **not happy** ❌

Return **TRUE** if `n` is happy, otherwise **FALSE**.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 2³¹ − 1`

---

## 🎯 Intuition

Every number either:
✔️ Reaches **1** eventually ⟶ Happy
❌ Falls into a repeating loop ⟶ Not Happy

To detect cycle formation, we have **two valid approaches**:

---

## 🔹Approach 1 — HashSet (Detect Repetition)

Track numbers already seen:

| Condition                             | Interpretation             |
| ------------------------------------- | -------------------------- |
| Reaches `1`                           | Happy                      |
| We see a previously seen number again | Cycle detected → Not happy |

---

### ✅ Java Solution (HashSet)

```java
import java.util.*;

public class Solution {
    public boolean isHappy(int n) {
        Set<Integer> visited = new HashSet<>();
        
        while (n != 1 && !visited.contains(n)) {
            visited.add(n);
            n = sumOfSquares(n);
        }
        
        return n == 1;
    }
    
    private int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
}
```
#### ⏱️ Complexity
- **Time Complexity** - O(log n)
- **Space Complexity** - O(log n)

---

## 🔹Approach 2 — Floyd’s Cycle Detection (Fast & Slow Pointers)

Same logic but **no extra memory** required.

Idea:

* Move `slow` one step each time
* Move `fast` two steps each time
* If they meet at `1` → Happy
* If they meet at any other number → Cycle → Not happy

---

### ✅ Java Solution (Floyd’s Algorithm)

```java
public class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = n;
        
        do {
            slow = sumOfSquares(slow);              // 1 step
            fast = sumOfSquares(sumOfSquares(fast)); // 2 steps
        } while (slow != fast);
        
        return slow == 1;
    }
    
    private int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
}
```

#### ⏱️ Complexity
- **Time Complexity** - O(log n)
- **Space Complexity** - O(1) *(No extra memory)* ✔️

## ⭐ Key Takeaways

| Feature        | HashSet | Floyd’s Cycle Detection |
| -------------- | ------- | ----------------------- |
| Detect cycles  | ✔️      | ✔️                      |
| Extra space    | ❌       | ✔️ Best choice          |
| Implementation | Easy    | Slightly tricky         |
