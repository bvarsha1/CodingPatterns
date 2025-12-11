## Sliding Window Maximum

#### **Statement**

You are given an array of integers nums and a sliding window of size w that moves from left to right across the array, shifting one position at a time.

Your task is to find the maximum value within the current window at each step and return it.

---

#### ✔️ Constraints

1 ≤ nums.length ≤ 10^3
−10^4 ≤ nums[i] ≤ 10^4
1 ≤ w ≤ nums.length

---

## 🎯 Intuition

**Brute Force Approach**

Brute force would check all elements inside each window → **O(n · w)**

**Sliding Window + Deque Approach**

When you slide a window across an array and need the **maximum** in every window, the naïve way is to look at all w elements each time — but that becomes too slow.

The trick is to maintain a data structure that always gives you the **current window’s maximum in O(1)**, and updates itself in **O(1) amortized** when the window moves.

#### 🔹 Key Idea

Use a **deque that stores indices of elements in *strictly decreasing* order of their values**.
This ensures:

* The **front** of the deque always holds the index of the **maximum element** of the current window.
* Every element in the deque is a “candidate” for being a future maximum.

### 🔹 Why remove elements from the back?

Before inserting a new element, you remove (from the back) all elements whose values are **less than or equal** to the new one.

Why?
Because:

* If a new element is **greater**, the smaller ones can never be maximum again — the new element will dominate them in all future windows.
* Keeping them would be useless and slow down operations.

So, the deque always remains sorted decreasingly by value.

#### 🔹 Why remove elements from the front?

Whenever the window moves forward, the element at the front might fall **out of the window’s left boundary**.
If the index at the front is outside the current window, we simply drop it.

#### 🔹 Putting it together

For each index:

1. **Pop from back** all indices with values ≤ current value — keep deque decreasing.
2. **Pop from front** if it's outside the window.
3. **Add the current index** to the deque.
4. The **front of the deque** is always the current window’s maximum.

This gives each element at most one push and one pop → **O(n) time** overall.

---

## ✅ Java Solution

```java
import java.util.*;

class SlidingWindowMaximum {
	public static int[] findMaxSlidingWindow(int[] nums, int w) {
    Deque<Integer> q = new ArrayDeque<>();
    for(int i = 0; i < w; i++) {
      // because we want to keep order strictly decreasing
      // because equal or lesser elements are useless
      while(!q.isEmpty() && nums[q.getLast()] <= nums[i]) {
        q.pollLast();
      }
      q.offer(i);
    }
    
    int[] output = new int[nums.length - w + 1];
    output[0] = nums[q.getFirst()];
    for(int i = w; i < nums.length; i++) {
      // maintain strictly decreasing here as well
      while(!q.isEmpty() && nums[q.getLast()] <= nums[i]) {
        q.pollLast();
      }
      
      // cleanup out of window bounds elements
      if(!q.isEmpty() && q.getFirst() <= (i - w)) q.pollFirst();
      q.offer(i);
      
      output[i - w + 1] = nums[q.getFirst()];
    }
    
    return output;
	}
}
```

**Cleaner 1 pass solution**
```java
import java.util.*;

class SlidingWindowMaximum {
	public static int[] findMaxSlidingWindow(int[] nums, int w) {
        Deque<Integer> dq = new ArrayDeque<>();
        int[] ans = new int[nums.length - w + 1];
        int idx = 0;
        
        for(int i = 0; i < nums.length; i++) {
            // check out of bounds
            while(!dq.isEmpty() && dq.getFirst() <= i - w) {
                dq.pollFirst();
            }
            
            // check decreasing sequence
            while(!dq.isEmpty() && nums[dq.getLast()] <= nums[i]) {
                dq.pollLast();
            }
            
            dq.offer(i);
            
            if(i >= w - 1) {
                ans[idx++] = nums[dq.peekFirst()];
            }
        }
        
        return ans;
	}
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(w)
