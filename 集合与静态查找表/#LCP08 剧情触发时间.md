[toc]

在战略游戏中，玩家往往需要发展自己的势力来触发各种新的剧情。一个势力的主要属性有三种，分别是文明等级（`C`），资源储备（`R`）以及人口数量（`H`）。在游戏开始时（第 $0$ 天），三种属性的值均为 $0$。

随着游戏进程的进行，每一天玩家的三种属性都会对应**增加**，我们用一个二维数组 `increase` 来表示每天的增加情况。这个二维数组的每个元素是一个长度为 $3$ 的一维数组，例如 `[[1,2,1],[3,4,2]]` 表示第一天三种属性分别增加 `1,2,1` 而第二天分别增加 `3,4,2`。

所有剧情的触发条件也用一个二维数组 `requirements` 表示。这个二维数组的每个元素是一个长度为 $3$ 的一维数组，对于某个剧情的触发条件 `c[i], r[i], h[i]`，如果当前 $C \ge \text{c[i]}$ 且 $R \ge \text{r[i]}$ 且 $H \ge \text{h[i]}$ ，则剧情会被触发。

根据所给信息，请计算每个剧情的触发时间，并以一个数组返回。如果某个剧情不会被触发，则该剧情对应的触发时间为 $-1$ 。



**限制**：

* $1 \le \text{increase.length} \le 10000$
* $1 \le \text{requirements.length} \le 100000$
* $0 \le \text{increase[i]} \le 10$
* $0 \le \text{requirements[i]} \le 100000$



## 题目解读

&emsp;计算剧情触发时间。

```java
class Solution {
    public int[] getTriggerTime(int[][] increase, int[][] requirements) {

    }
}
```

## 程序设计

* 采用二分查找查找满足条件的最小天数。

```java
class Solution {
    public int[] getTriggerTime(int[][] increase, int[][] requirements) {
        int n = increase.length, m = requirements.length;
        // 叠加计算
        for (int i = 1; i < n; i++) {
            increase[i][0] += increase[i - 1][0];
            increase[i][1] += increase[i - 1][1];
            increase[i][2] += increase[i - 1][2];
        }

        int[] res = new int[m];
        // 遍历查找
        for (int i = 0; i < m; i++) {
            // 特殊情况
            if (requirements[i][0] == 0 && requirements[i][1] == 0 && requirements[i][2] == 0) res[i] = 0;
            // 二分查找
            else {
                int left = 0, right = n;
                while (left < right) {
                    int mid = left + (right - left) / 2;
                    // mid满足条件，继续向下尝试
                    if (requirements[i][0] <= increase[mid][0] && requirements[i][1] <= increase[mid][1] 
                        && requirements[i][2] <= increase[mid][2]) right = mid;
                    else left = mid + 1;
                }
                // left为满足条件的最小索引，天数为left+1
                if (left == n) res[i] = -1;
                else res[i] = left + 1;
            }
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M\log_2N)$，空间复杂度为$O(1)$。

执行用时：43ms，在所有java提交中击败了83.56%的用户。

内存消耗：95.2MB，在所有java提交中击败了19.23%的用户。

## 官方解题

&emsp;官方思路类似。