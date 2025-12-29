## Nested List Weight Sum II

#### Statement

Given a nested list of integers, `nestedList`, where each element can either be an integer or a list, which can contain integers or more lists, return the sum of each integer in `nestedList` multiplied by its weight.

The weight of an integer is calculated as **`maxDepth − depth + 1`**.

The depth of an integer is defined as the number of nested lists it is contained within. For instance, the value of each integer in the list
`[1,[2,2],[[3],2],1]` is equal to its depth.
Let `maxDepth` represent the maximum depth of any integer in the `nestedList`.

---

#### ✔️ Constraints

* `1 ≤ nestedList.length ≤ 50`
* `−100 ≤ integer value ≤ 100`
* `maxDepth ≤ 50`

---

## 🎯 Intuition

This problem is the **inverse** of the classic *Nested List Weight Sum*.

Instead of:

```
weight = depth
```

We use:

```
weight = maxDepth - depth + 1
```

### Key challenge

👉 We **don’t know `maxDepth` upfront**.

### Two common approaches

1. **Two-pass DFS**

   * First pass → find `maxDepth`
   * Second pass → compute weighted sum
2. **One-pass BFS (preferred & elegant)** ⭐

---

## 🧠 One-pass BFS Insight (Why it works)

While doing a **level-order traversal**:

* Maintain:

  * `unweightedSum` → sum of all integers seen so far
  * `weightedSum` → final answer
* At each level:

  * Add integers at this level to `unweightedSum`
  * Add `unweightedSum` to `weightedSum`

Why this works:

* Shallow integers get added **more times**
* Deeper integers get added **fewer times**
* This naturally simulates
  `weight = maxDepth - depth + 1`

✨ No need to explicitly compute `maxDepth`.

---

## ✅ Java Solution (BFS – One Pass)

```java
import java.util.*;

// This is the interface provided by the problem
// interface NestedInteger {
//     boolean isInteger();
//     Integer getInteger();
//     List<NestedInteger> getList();
// }

public class Solution {
    public static int depthSumInverse(List<NestedInteger> nestedList) {
        Queue<NestedInteger> queue = new LinkedList<>(nestedList);
        int unweightedSum = 0;
        int weightedSum = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();

            for (int i = 0; i < size; i++) {
                NestedInteger ni = queue.poll();

                if (ni.isInteger()) {
                    unweightedSum += ni.getInteger();
                } else {
                    for (NestedInteger child : ni.getList()) {
                        queue.offer(child);
                    }
                }
            }

            // add current cumulative sum
            weightedSum += unweightedSum;
        }

        return weightedSum;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each integer/list processed once

* **Space Complexity:** `O(n)`
  * Queue for BFS traversal

---

## ✅ Java Solution: (DFS - One pass)

```java
/**
 * // This interface facilitates the creation of nested lists
 * // Do not implement or make assumptions about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer rather than a nested list
 *     public boolean isInteger();
 *
 *     // @return the single integer this NestedInteger holds, if it holds a single integer
 *     // Otherwise, return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Sets this NestedInteger to hold a single integer equal to value
 *     public void setInteger(int value);
 *
 *     // Sets this NestedInteger to hold a nested list and adds the nested integer elem to it
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Otherwise, return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
 
import java.util.List;

public class Solution {
    public int weightedDepthSum(List<NestedInteger> nestedList) {
        // find maxDepth
        int maxDepth = dfsMaxDepth(nestedList, 1);
        
        // calculated weightedSum using formula (maxDepth - depth + 1)
        return dfsWeightedSum(nestedList, 1, maxDepth);
    }
    
    public int dfsMaxDepth(List<NestedInteger> nestedList, int depth) {
        int maxDepth = depth;
        
        for(NestedInteger ni : nestedList) {
            if(!ni.isInteger()) {
                maxDepth = Math.max(maxDepth, dfsMaxDepth(ni.getList(), depth + 1));
            }
        }
        
        return maxDepth;
    }
    
    public int dfsWeightedSum(List<NestedInteger> nestedList, int depth, int maxDepth) {
        int sum = 0;
        
        for(NestedInteger ni : nestedList) {
            if(ni.isInteger()) {
                sum += (maxDepth - depth + 1) * ni.getInteger();
            } else {
                sum += dfsWeightedSum(ni.getList(), depth + 1, maxDepth);
            }
        }
        
        return sum;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n)`
  * Each integer/list processed once

* **Space Complexity:** `O(d)`
  * recursion stack, where d is the maximum depth of the nested list