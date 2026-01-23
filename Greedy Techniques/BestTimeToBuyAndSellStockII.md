## Best Time to Buy and Sell Stock II

#### **Statement**

You are given an integer array, `prices`, where `prices[i]` is the price of a stock on the i-th day.

You may not hold more than one share at a time, and you must sell your stock before you can buy again. However, you can sell and buy the stock multiple times on the same day, as long as you never hold more than one share at any moment.

Find the maximum profit you can achieve by completing as many transactions as you like (i.e., buying one and selling one share of the stock multiple times).

---

#### ✔️ Constraints

* 1 ≤ `prices.length` ≤ 3 × 10^3
* 0 ≤ `prices[i]` ≤ 10^4

---

## 🎯 Intuition

This is a classic **Greedy** problem.

Key idea:

* Profit is gained from every **ascending price movement**
* Whenever `prices[i] > prices[i-1]`, we collect that profit difference
* This simulates buying before the rise and selling at the high point

Approach:

1. Initialize profit = 0
2. Iterate through array from day 1 to end
3. If price increased compared to previous day:

   * Add the difference to profit
4. Return total profit

This strategy yields the maximum possible profit with unlimited transactions.

---

## ✅ Java Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i - 1];
            }
        }
        
        return profit;
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
