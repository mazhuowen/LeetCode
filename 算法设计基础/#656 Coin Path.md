[toc]

Given an array `A` (index starts at $1$) consisting of $N$ integers: `A1, A2, ..., AN` and an integer `B`. The integer `B` denotes that from any place (suppose the index is i) in the array `A`, you can jump to any one of the place in the array `A` indexed `i+1, i+2, …, i+B` if this place can be jumped to. Also, if you step on the index $i$, you have to pay `Ai` coins. If `Ai` is $-1$, it means you can’t jump to the place indexed $i$ in the array.

Now, you start from the place indexed $1$ in the array `A`, and your aim is to reach the place indexed $N$ using the minimum coins. You need to return the path of indexes (starting from $1$ to $N$) in the array you should take to get to the place indexed $N$ using minimum coins.

If there are multiple paths with the same cost, return the lexicographically smallest such path.

If it's not possible to reach the place indexed $N$ then you need to return an empty array.



**Example 1**:

```
Input: [1,2,4,-1,2], 2
Output: [1,3,5]
```

**Example 2**:

```
Input: [1,2,4,-1,2], 1
Output: []
```



**Note**:

* Path `Pa1, Pa2, ..., Pan` is lexicographically smaller than `Pb1, Pb2, ..., Pbm`, if and only if at the first $i$ where `Pai` and `Pbi` differ, `Pai < Pbi`; when no such $i$ exists, then $n < m$.
* $A_1 \ge 0$. `A2, ..., AN` (if exist) will in the range of $[-1, 100]$.
* Length of `A` is in the range of $[1, 1000]$.
* `B` is in the range of $[1, 100]$.



## 题目解读

&emsp;找到到达终点的花费最小的路径，如果存在多条路径，则返回字典序最小的。

```java
class Solution {
    public List<Integer> cheapestJump(int[] A, int B) {
        
    }
}
```

## 程序设计

* 初步思路是使用动态规划`dp(i)`，表示到达$i$的最小代价，则路径就是最后根据花费的回溯；这个思路有个问题，假设得到的路径是`[1, 3, 5]`，而$1 \sim 3$的索引间存在值为$0$的点，这个点显然是可加入路径的，且字典序更小；
* 该思路问题之二是在生成路径时，比如`[1, 3, 5]`而不是`[1, 2, 4, 5]`，但是字典序显然是后一个更小；这个问题可使用回溯生成所有的路径，进行对比得到。

```java
class Solution {
    public List<Integer> cheapestJump(int[] A, int B) {
        List<Integer> res = new LinkedList<>();
        if (A == null || A.length == 0) return res;

        // 动态规划判断最小代价
        int n = A.length;
        int[] dp = new int[n];
        Arrays.fill(dp, Integer.MAX_VALUE / 2);
        dp[0] = A[0];
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j <= i + B && j < n; j++) {
                if (A[j] == -1) continue;
                dp[j] = Math.min(dp[j], dp[i] + A[j]);
            }
        }
        if (dp[n - 1] == Integer.MAX_VALUE / 2) return res;
        // 回溯获得最小字典序路径
        getPath(n - 1, dp, new LinkedList<>(), A, B);

        // 填充路径中的0值
        tmp.add(0, 1);
        res.add(tmp.get(0));
        for (int i = 1; i < tmp.size(); i++) {
            int pre = tmp.get(i - 1), cur = tmp.get(i);
            for (int j = pre; j < cur - 1; j++) {
                if (A[j] == 0) res.add(j + 1);
            }
            res.add(cur);
        }
        return res;
    }

    List<Integer> tmp = new LinkedList<>();
    private void getPath(int i, int[] dp, List<Integer> path, int[] A, int B) {
        if (i == 0) {
            // 有更小的字典序，替换
            if (tmp.isEmpty() || less(tmp, path)) tmp = new LinkedList<>(path);
            return;
        }
        
        path.add(0, i + 1);
        for (int j = 1; j <= B && j <= i; j++) {
            if (dp[i - j] != Integer.MAX_VALUE / 2 && dp[i] == dp[i - j] + A[i]) {
                getPath(i - j, dp, path, A, B);
            }
        }
        path.remove(0);
    }

    private boolean less(List<Integer> l1, List<Integer> l2) {
        int idx1 = 0, idx2 = 0;
        while (idx1 < l1.size() && idx2 < l2.size()) {
            if (l1.get(idx1) > l2.get(idx2)) return true;
            else if (l1.get(idx1) < l2.get(idx2)) return false;
            idx1++;
            idx2++;
        }
        return idx2 == l2.size();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NB)$，空间复杂度为$O(N)$。

执行用时：8 ms, 在所有 Java 提交中击败了16.00%的用户。

内存消耗：38.8 MB, 在所有 Java 提交中击败了24.00%的用户。

## 官方解题

&emsp;官方采用从后往前迭代的动态规划，从而省去遍历所有路径的花销。

```java
class Solution {
    public List<Integer> cheapestJump(int[] A, int B) {
        List<Integer> res = new LinkedList<>();
        if (A == null || A.length == 0) return res;

        int n = A.length;
        // i~n-1的最小代价，保存其后继（代价相同则选择最近的一个，字典序小）
        int[] next = new int[n];
        int[] dp = new int[n];

        // 从后往前
        for (int i = n - 2; i >= 0; i--) {
            int min = Integer.MAX_VALUE / 2;
            // 从前往后
            for (int j = i + 1; j <= i + B && j < n; j++) {
                if (A[j] == -1) continue;
                // 当等于时，原先的索引小于当前索引，字典序较小，故不更新
                if (min > A[i] + dp[j]) {
                    min = A[i] + dp[j];
                    next[i] = j;
                }
            }
            dp[i] = min;
        }

        if (dp[0] == Integer.MAX_VALUE / 2) return res;
        int idx = 0;
        while (idx < n - 1) {
            res.add(idx + 1);
            idx = next[idx];
        }
        res.add(n);
        return res;
    }
}
```

&emsp;时间复杂度为$O(NB)$，空间复杂度为$O(N)$。

执行用时：4 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：38.5 MB, 在所有 Java 提交中击败了75.00%的用户。