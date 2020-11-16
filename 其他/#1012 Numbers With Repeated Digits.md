[toc]

Given a positive integer $N$, return the number of positive integers less than or equal to $N$ that have at least $1$ repeated digit.



**Note:**

* $1 \le N \le 10^9$



## 题目解读

&emsp;返回小于等于给定数字且只有有一位重复的数字个数。

```java
class Solution {
    public int numDupDigitsAtMostN(int N) {

    }
}
```

## 程序设计

* 参考社区思路，是典型的数位动态规划。动态规划数组`dp(pos,stat,lead,limit)`，其中：
  * `pos`表示当前的数字位，从最高位到最低位回溯尝试；
  * `stat`表示最高位到`pos`前的数字状态，本题状态为记录数字的使用情况，使用长度为$10$的二进制数字表示；
  * `lead`表示前导是否为$0$，值为$1$或$0$；当`lead`为$1$，当前位置值为$0$，则`lead`仍然为$1$，否则为$0$；
  * `limit`表示是否是上界，$0$表示不是，取值范围为$0 \sim 9$，$1$表示是，取值范围为$0 \sim num$。
* `dp(pos,stat,lead,limit)`表示位置`pos`之后符合要求的计数，最后结果就是`dp(0,0,true,true)`，对于状态`stat`记录`pos`前的数字，当前的`pos`位置不能使用之前已经使用过的数字。
* 最后得到不存在重复数字的数目，然后减去这个数字得到结果。

```java
class Solution {
    // pos,stat,lead,limit
    int[][][][] record = new int[10][1 << 10][2][2];
    // high
    char[] high;
    public int numDupDigitsAtMostN(int N) {
        // 初始化记忆数组
        for (int[][][] r : record) {
            for (int[][] s : r) {
                for (int[] t : s) {
                    Arrays.fill(t, -1);
                }
            }
        }
        high = Integer.toString(N).toCharArray();
        // 由于dfs的计数包含了0，加一抵消
        return 1 + N - dfs(0, 0, true, true);
    }

    private int dfs(int pos, int preStat, boolean lead, boolean limit) {
        // 遍历完一个数字，返回1
        if (pos >= high.length) return 1;
        // 记忆化搜索
        if (record[pos][preStat][lead ? 1 : 0][limit ? 1 : 0] != -1) return record[pos][preStat][lead ? 1 : 0][limit ? 1 : 0];

        int res = 0;
        int h = limit ? high[pos] - '0' : 9;
        for (int num = 0; num <= h; num++) {
            // 当前状态
            int curStat = preStat | (1 << num);
            // 当前数字已存在（需要额外判断前导为0且当前也为0的情况，不能拦截）
            if (!(lead && num == 0) && curStat == preStat) continue;
            // 注意前导为0且当前为0的情况状态为preStat，避免高位无效位将0置为1，导致后续无法选择0
            res += dfs(pos + 1, lead && num == 0 ? preStat : curStat, lead && num == 0, limit && high[pos] - '0' == num);
        }

        record[pos][preStat][lead ? 1 : 0][limit ? 1 : 0] = res;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(L!)$，空间复杂度为$O(L)$，其中$L$为数字位数。

执行用时：187ms，在所有java提交中击败了5.38%的用户。

内存消耗：42.4MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用排列组合的思路，以`5472`为例，首先计算一位的不重复数字，显然有$1 \sim 9$共$9$个，然后计算两位的不重复数字，高位只能选择$1 \sim 9$，低位选择除去高位的$9$个数字，共$9 \times 9 = 81$个数字；三位的同理，共$9 \times 9 \times 8 = 648$个。可见对于长度为$L$的数字，$i \in 1 \sim L - 1$位数字拥有的不重复数字为$9 \times 9 \times 8 \times \dots \times (11 - i)$。问题的关键在于长度为$L$有多少非重复数字，长度为$L$的数字分在边界上及在边界内两种情况，从最高位开始遍历，最高位可选择$1 \sim 5$，此时分两种情况：即最高位选择$1 \sim 4$，则之后可选择$0 \sim 9$，有$4 \times 9 \times 8 \times 7 = 2016$个；其次最高位选择$5$，次高位选择范围位$0 \sim 4$，问题又转化位上述步骤，只是位数判断是次高位，重复该过程，直到遍历完所有位数。

```java
class Solution {
    int[] f = new int[10];

    public int numDupDigitsAtMostN(int N) {
        char[] nums = Integer.toString(N).toCharArray();
        int len = nums.length, total = 0;
        // 1~len-1位不重复数字数目
        for (int i = 1; i < len; i++) {
            total += 9 * f(9, i - 1);
        }

        // 记录边界数字使用情况
        boolean[] used = new boolean[10];
        // 计算len位的数目
        for (int i = len; i >= 1; i--) {
            int h = nums[len - i] - '0';
            // 第i位不是最大边界值的情况（最高位不能为0）
            for (int j = i == len ? 1 : 0; j < h; j++) {
                // 已使用
                if (used[j]) continue;
                // 前len - i + 1位是边界数字，已确定，后续数字需要除去边界数字计算排列组合
                total += f(10 - (len - i + 1), i - 1);
            }
            // 第i位是边界数字的情况
            if (used[h]) break;
            used[h] = true;
            // 边界数字，即N若没有重复，则加一
            if (i == 1) total += 1;
        }
        return N - total;
    }

    // 完全阶乘
    private int factorial(int num) {
        if (num <= 1) return 1;
        if (f[num] != 0) return f[num];
        return f[num] = num * factorial(num - 1);
    }

    // 不完全阶乘
    private int f(int num, int time) {
        return factorial(num) / factorial(num - time);
    }
}
```

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(L)$。

执行用时：1ms，在所有java提交中击败了98.46%的用户。

内存消耗：36.3MB，在所有java提交中击败了85.00%的用户。