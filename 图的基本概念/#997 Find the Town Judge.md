[toc]

In a town, there are `N` people labelled from `1` to `N`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

* The town judge trusts nobody.
* Everybody (except for the town judge) trusts the town judge.
* There is exactly one person that satisfies properties 1 and 2.

You are given `trust`, an array of pairs `trust[i] = [a, b]` representing that the person labelled `a` trusts the person labelled `b`.

If the town judge exists and can be identified, return the label of the town judge. Otherwise, return `-1`.

**Note:**

* $1 \le N \le 1000$
* $\text{trust.length} \le 10000$
* $\text{trust[i]}$ are all different
* $\text{trust[i][0]} \ne \text{trust[i][1]}$
* $1 \le \text{trust[i][0], trust[i][1]} \le N$



## 题目解读

&emsp;题目规定除了法官所有人都信任法官，法官不信任任何人，查找给定的关系是否满足条件。题目规定边是不重复的。

```java
class Solution {
    public int findJudge(int N, int[][] trust) {

    }
}
```

## 程序设计

* 根据题目描述，所有人都指向法官，而法官出边为0，则就是找出入度为`N - 1`，出度为`0`的结点，且这个点只有一个。

```java
class Solution {
    public int findJudge(int N, int[][] trust) {
        if (N <= 0 || trust == null) return -1;
        // 统计出度、入度
        int[] outDegree = new int[N];
        int[] inDegree = new int[N];
        for (int[] edge : trust) {
            outDegree[edge[0] - 1]++;
            inDegree[edge[1] - 1]++;
        }
        // 有且只有一个
        int idx = -1;
        for (int i = 0; i < N; i++) {
            if (outDegree[i] == 0 && inDegree[i] == (N - 1)) {
                if (idx == -1) idx = i + 1;
                else return -1;
            }
        }
        return idx;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V)$。

执行用时：3ms，在所有java提交中击败了83.15%的用户。

内存消耗：48.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。