## Sort an Array

#### **Statement**

Given an integer array, `nums`, sort it in ascending order and return the sorted array.

You must implement the sorting algorithm yourself; do not use any built-in sorting functions. The solution must run in
O(nlogn) time and use the minimum possible extra space.

---

#### ✔️ Constraints

* 1 ≤ `nums.length` ≤ 10^3
* −10^3 ≤ `nums[i]` ≤ 10^3

---

## 🎯 Intuition

To achieve O(nlogn) time without using built-in sorting, two classical algorithms fit well:

* **Merge Sort** — stable but uses O(n) extra space
* **Heap Sort** — in-place with O(1) extra space but less stable

Since the problem asks for "minimum possible extra space", **Heap Sort** is the ideal choice.

**Heap Sort Steps:**

1. Build a max heap from the array
2. Repeatedly swap the first element (max) with the last unsorted element
3. Reduce heap size and heapify again

This ensures the array is sorted in ascending order.

---

## ✅ Java Solution (Heap Sort)

```java
import java.util.*;

public class Solution {
    
    public int[] sortArray(int[] nums) {
        int n = nums.length;
        
        for(int i = n / 2 - 1; i >= 0; i--) {
            heapify(nums, n, i);
        }
        
        for(int i = n - 1; i >= 0; i--) {
            int temp = nums[i];
            nums[i] = nums[0];
            nums[0] = temp;
            
            heapify(nums, i, 0);
        }
        
        return nums;
    }
    
    public void heapify(int[] nums, int n, int i) {
        while(true) {
            int largest = i;
            int left = 2 * i + 1;
            int right = 2 * i + 2;
            
            if(left < n && nums[left] > nums[largest]) largest = left;
            if(right < n && nums[right] > nums[largest]) largest = right;
            
            if(largest == i) break;
            
            int temp = nums[largest];
            nums[largest] = nums[i];
            nums[i] = temp;
            
            i = largest;
        }
    }
}
```

#### ⏱️ Complexity

* **Time Complexity:** O(n log n)
* **Space Complexity:** O(1)