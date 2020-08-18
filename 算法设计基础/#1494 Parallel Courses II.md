[toc]

Given the integer $n$ representing the number of courses at some university labeled from $1$ to $n$, and the array dependencies where `dependencies[i] = [xi, yi]`  represents a prerequisite relationship, that is, the course $x_i$ must be taken before the course $y_i$.  Also, you are given the integer $k$.

In one semester you can take **at most** $k$ courses as long as you have taken all the prerequisites for the courses you are taking.

Return the minimum number of semesters to take all courses. It is guaranteed that you can take all courses in some way.



**Constraints**:

* $1 \le n \le 15$
* $1 \le k \le n$
* $0 \le \text{dependencies.length} \le n * (n-1) / 2$
* $\text{dependencies[i].length} == 2$
* $1 \le x_i, y_i \le n$
* $x_i \ne y_i$
* All prerequisite relationships are distinct, that is, $\text{dependencies[i]} \ne \text{dependencies[j]}$.
* The given graph is a directed acyclic graph.



## 题目解读

&emsp;给定课程依赖关系，每学期限定最多上$k$门课，求学完所有课程所需的最少学期数。

```java
class Solution {
    public int minNumberOfSemesters(int n, int[][] dependencies, int k) {
        
    }
}
```

## 程序设计

* 粗看似乎是拓扑排序的问题，题目中也规定无环；仔细分析由于限定每学期可上课程上限，其次依赖关系也可以是森林而不是单一的树，这使得简单拓扑排序无法适用。
* 由于题目规模较少，可使用回溯暴力遍历，遍历过程中需要记录已学课程，可发现这是路径状态压缩动态规划问题，使用二进制数字表示已学课程$stat$，提前将课程的依赖课程表示为二进制字符串$depend$，这样问题转化为$stat\ \&\ depend == depend$。
* 使用动态规划数组表示当前课程学习路径所需的最少学期数。得到当前可学的候选课程后判断数目是否大于$k$，大于$k$则需要遍历尝试最佳方案。

```java
class Solution {
    public int minNumberOfSemesters(int n, int[][] dependencies, int k) {
        if (dependencies == null || dependencies.length == 0 || k == 1) return n / k  + (n % k == 0 ? 0 : 1);

        // 每个课程依赖的二进制表示
        int[] dependStat = new int[n];
        for (int[] denpend : dependencies) {
            dependStat[denpend[1] - 1] |= 1 << (denpend[0] - 1);
        }

        // 动态规划数组，表示每个状态的最低学期数
        int[] dp = new int[1 << n];
        Arrays.fill(dp, n);
        // 初始化，不上课学期数为0
        dp[0] = 0;
        for (int stat = 0; stat < (1 << n); stat++) {
            // 当前可学课程二进制表示
            int candidate = 0;
            for (int course = 0; course < n; course++) {
                int depend = dependStat[course];
                // 当前课程未学习且其依赖课程已学习，即当前课程可学
                if ((stat & (1 << course)) == 0 && (depend & stat) == depend) {
                    candidate |= 1 << course;
                }
            }
            // 可学课程数目小于等于k，则直接学习
            if (Integer.bitCount(candidate) <= k) dp[stat | candidate] = Math.min(dp[stat | candidate], dp[stat] + 1);
            // 可学课程超过k个，需要遍历尝试所有组合
            else {
                // sub - 1低位全部为1，与candidate与趋势为高位的1逐渐变为0，1在低位出现，直到为0
                // 迭代得到所有组合数
                for (int sub = candidate; sub > 0; sub = (sub - 1) & candidate) {
                    if (Integer.bitCount(sub) <= k) dp[stat | sub] = Math.min(dp[stat | sub], dp[stat] + 1);
                }
            }
        }
        return dp[(1 << n) - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(N2^N, 3^N))$，空间复杂度为$O(2^N)$。

执行用时：71ms，在所有java提交中击败了35.69%的用户。

内存消耗：39.4MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。