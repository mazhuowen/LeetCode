[toc]

小扣注意到秋日市集上有一个创作黑白方格画的摊位。摊主给每个顾客提供一个固定在墙上的白色画板，画板不能转动。画板上有 $n * n$ 的网格。绘画规则为，小扣可以选择任意多行以及任意多列的格子涂成黑色，所选行数、列数均可为 $0$。

小扣希望最终的成品上需要有 $k$ 个黑色格子，请返回小扣共有多少种涂色方案。

注意：两个方案中任意一个相同位置的格子颜色不同，就视为不同的方案。



**限制：**

- $1 \le n \le 6$
- $0 \le k \le n * n$



## 题目解读

&emsp;每次只能涂一行或一列，求有$k$个黑格的涂色方案。

```java
class Solution {
    public int paintingPlan(int n, int k) {

    }
}
```

## 程序设计

* 由于只能涂一整行或一整列，故当涂的行数、列数固定时，假设为`row`、`col`，则数目为$k = row * n + col * (n - row)$是固定的，这样可以枚举所有的行列涂色数目，然后通过组合方式得到最后的答案。

```java
class Solution {
    public int paintingPlan(int n, int k) {
        if (k > n * n) return 0;
        // 不涂色也是一种方案
        if (k == n * n || k == 0) return 1;

        int res = 0;
        // 涂row行，col列的数目
        for (int row = 0; row < n; row++) {
            int num = k - row * n;
            if (num < 0) break;
            if (num % (n - row) == 0) {
                int col = num / (n - row);
                // 排列组合
                res += permutation(n, row) * permutation(n, col);
            }
        }
        return res;
    }

    private int permutation(int n, int m) {
        if (m <= 0 || m >= n) return 1;
        return permutation(n - 1, m) + permutation(n - 1, m - 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.4MB，在所有java提交中击败了76.69%的用户。

## 官方解题

&emsp;暂无，密切关注。