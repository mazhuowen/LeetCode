[toc]

You are given a list of `preferences` for $n$ friends, where $n$ is always **even**.

For each person $i$, `preferences[i]` contains a list of friends **sorted** in the **order of preference**. In other words, a friend earlier in the list is more preferred than a friend later in the list. Friends in each list are denoted by integers from $0$ to $n-1$.

All the friends are divided into pairs. The pairings are given in a list `pairs`, where $\text{pairs[i]} = [x_i, y_i]$ denotes $x_i$ is paired with $y_i$ and $y_i$ is paired with $x_i$.

However, this pairing may cause some of the friends to be unhappy. A friend `x` is unhappy if `x` is paired with `y` and there exists a friend `u` who is paired with `v` but:

* `x` prefers `u` over `y`, and
* `u` prefers `x` over `v`.

Return the number of unhappy friends.



**Constraints**:

* $2 \le n \le 500$
* $n$ is even.
* $\text{preferences.length} == n$
* $\text{preferences[i].length} == n - 1$
* $0 \le \text{preferences[i][j]} \le n - 1$
* `preferences[i]` does not contain $i$.
* All values in `preferences[i]` are unique.
* $\text{pairs.length} == \frac{n}{2}$
* $\text{pairs[i].length} == 2$
* $x_i \ne y_i$
* $0 \le x_i, y_i \le n - 1$
* Each person is contained in **exactly one** pair.



## 题目解读

&emsp;给定`preferences`，表示每个人和其他人亲近程度由高到低的列表；`pairs`表示分派的朋友对，根据题目要求，求不开心的人的数目。

```java
class Solution {
    public int unhappyFriends(int n, int[][] preferences, int[][] pairs) {

    }
}
```

## 程序设计

* 对于关系`(x,y)`，如果能找到另一个关系`(u,v)`，且`xu`、`ux`的优先级高于`xy`、`uv`，则`x`不开心；判断这种关系需要遍历判断当前关系对`(x,y)`中`x`后的关系列表，判断是否存在`u`在`y`前面，且`x`在`v`前面；
* 由于题目限定关系对中的朋友都是唯一的，既没有重复，可使用映射表映射；对于判断关系是否在前面，需要遍历`preferences`判断，可提前将`preferences`数组转化为索引表示，这样只需对比索引的先后。

```java
class Solution {
    public int unhappyFriends(int n, int[][] preferences, int[][] pairs) {
        // 将亲近关系转化为根据人的编号的索引数组，index[i][j]=k表示j是i的第k顺位的朋友
        int[][] index = new int[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n - 1; j++) {
                index[i][preferences[i][j]] = j;
            }
        }
        // 由于不存在重复朋友对，映射到数组
        int[] relation = new int[n];
        for (int[] pair : pairs) {
            relation[pair[0]] = pair[1];
            relation[pair[1]] = pair[0];
        }

        int res = 0;
        // 对比每个人的朋友
        for (int i = 0; i < n; i++) {
            for (int j : preferences[i]) {
                // 存在x和u优先于原有关系
                if (index[i][j] < index[i][relation[i]] && index[j][i] < index[j][relation[j]]) {
                    res++;
                    break;
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。



## 官方解题

&emsp;