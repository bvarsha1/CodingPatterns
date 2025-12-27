## Find the Town Judge

#### Statement

There are `n` people numbered from `1` to `n` in a town. There’s a rumor that one of these people is secretly the town judge. A town judge must meet the following conditions:

* The judge doesn’t trust anyone.
* Everyone else in the town (except the town judge) trusts the judge.
* There is exactly one person who fulfills both the above conditions.

You are given an integer `n` and a two-dimensional array, `trust`, where each entry
`trust[i] = [ai, bi]` indicates that the person labeled `ai` trusts the person labeled `bi`.

If a trust relationship is not specified in the trust array, it does not exist.
Your task is to determine the label of the town judge, if one can be identified. If no such person exists, return `-1`.

---

#### ✔️ Constraints

* `1 ≤ n ≤ 1000`
* `0 ≤ trust.length ≤ 10^4`
* `trust[i].length = 2`
* `ai != bi`
* `1 ≤ ai, bi ≤ n`
* All the pairs in `trust` are unique

---

## 🎯 Intuition

We translate the judge’s conditions into **graph properties**:

* If person `a` trusts person `b`, that is a **directed edge** `a → b`
* The **judge** must have:

  * **Outdegree = 0** → trusts no one
  * **Indegree = n − 1** → trusted by everyone else

So the idea is:

1. Track how many people trust each person (**incoming edges**)
2. Track how many people each person trusts (**outgoing edges**)
3. Scan all people and find the one with:

   * `incoming == n - 1`
   * `outgoing == 0`

If such a person exists, they are the judge.

---

## ✅ Java Solution

```java
import java.util.*;

public class Solution {
    public static int findJudge(int n, int[][] trust) {
        int[] incoming = new int[n + 1];
        int[] outgoing = new int[n + 1];

        // Build indegree and outdegree counts
        for (int[] t : trust) {
            outgoing[t[0]]++;
            incoming[t[1]]++;
        }

        // Judge check
        for (int i = 1; i <= n; i++) {
            if (incoming[i] == n - 1 && outgoing[i] == 0) {
                return i;
            }
        }

        return -1;
    }
}
```

#### ⏱ Complexity

* **Time Complexity:** `O(n + trust.length)`
* **Space Complexity:** `O(n)`
