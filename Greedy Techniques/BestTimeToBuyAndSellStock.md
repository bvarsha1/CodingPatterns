## Best Time to Buy and Sell Stock

#### **Statement**

Given an array `prices`, where `prices[i]` represent the price of a stock on the i-th day, maximize profit by selecting a single day to buy the stock and a different day in the future to sell it.

Return the maximum profit that can be achieved from this transaction. If no profit can be made, return 0.

---

#### ✔️ Constraints

* We can’t sell before buying a stock, that is, the array index at which stock is bought will always be less than the index at which the stock is sold.
* 1 ≤ `prices.length` ≤ 10^3
* 0 ≤ `prices[i]` ≤ 10^5

---

## 🎯 Intuition

This problem can be solved using a **Greedy single pass** approach:

* Keep track of the **minimum price seen so far** (`minPrice`)
* At each day, calculate potential profit: `currentProfit = prices[i] - minPrice`
* Update `maxProfit` whenever `currentProfit` is higher
* This works because buying at the lowest price so far guarantees the highest profit up to that point.

---

## ✅ Java Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for (int price : prices) {
            if (price < minPrice) {
                minPrice = price;
            } else {
                maxProfit = Math.max(maxProfit, price - minPrice);
            }
        }

        return maxProfit;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)