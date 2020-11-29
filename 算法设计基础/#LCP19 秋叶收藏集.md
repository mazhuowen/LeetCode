[toc]

小扣出去秋游，途中收集了一些红叶和黄叶，他利用这些叶子初步整理了一份秋叶收藏集 `leaves`， 字符串 `leaves` 仅包含小写字符 `r` 和 `y`， 其中字符 `r` 表示一片红叶，字符 `y` 表示一片黄叶。
出于美观整齐的考虑，小扣想要将收藏集中树叶的排列调整成「红、黄、红」三部分。每部分树叶数量可以不相等，但均需大于等于 $1$。每次调整操作，小扣可以将一片红叶替换成黄叶或者将一片黄叶替换成红叶。请问小扣最少需要多少次调整操作才能将秋叶收藏集调整完毕。



**提示：**

- $3 \le \text{leaves.length} \le 10^5$
- `leaves` 中只包含字符 `'r'` 和字符 `'y'`



## 题目解读

&emsp;给定收集树叶序列，求交换为红、黄、红的最少操作数。

```java
class Solution {
    public int minimumOperations(String leaves) {

    }
}
```

## 程序设计

* 参考社区思路，由于最后结构必然是`红黄红`的结构，假设第一段红的结构索引为$[0,i)$，第三段红的结构索引为$[j,n)$，则最后的替换数目就是第一段、第三段中的黄叶子和第二段中的红叶子之和，只需在所有可能中选择替换数最少的那种；
* 如果根据红叶子统计区间和，`preSum[i]`表示$[0,i)$中包含的红叶子数，则需要转换黄叶子为红叶子的数目为$i-preSum[i]$；$[j,n)$的红叶子数目为$preSum[n]-preSum[j]$，需要转换黄叶子为红叶子的数目为$n-j-preSum[n]+preSum[j]$；$[i,j)$中红叶子数目为$preSum[j]-preSum[i]$，需要转换红叶子为黄叶子；共$n+preSum[n] + i-2*preSum[i] - (j-2*preSum[j])$；
* 可见上述都出现了结构$i - 2 * preSum[i]$，可遍历过程中记录之前最小的$i - 2 * preSum[i]$，并代入上式得到最小结果。

```java
class Solution {
    public int minimumOperations(String leaves) {
        char[] seq = leaves.toCharArray();
        int n = seq.length;
        int[] preSum = new int[n + 1];
        // 统计前缀和
        for (int i = 1; i <= n; i++) preSum[i] = preSum[i - 1] + (seq[i - 1] == 'r' ? 1 : 0);
        
        // min记录1~n-2中的i的最小结果
        int res = Integer.MAX_VALUE, min = 1 - 2 * preSum[1];
        // 遍历2~n-1中的j并计算
        for (int j = 2; j < n; j++) {
            // 当前值
            int cur = j - 2 * preSum[j];
            // 将之前最小值代入计算
            res = Math.min(res, n - preSum[n] + min - cur);
            // 更新最小值
            min = Math.min(min, cur);
        }
        return res;
    }
}
```

* 动态规划的思路为抽象`红黄红`的三个拼接阶段，即只有红的阶段、红黄的阶段、红黄红的阶段，这三个阶段根据当前拼接字符，状态会发生相互转换；
* 需注意初始化问题，由前两个字符决定三个状态的初始值。

```java
class Solution {
    public int minimumOperations(String leaves) {
        char[] seq = leaves.toCharArray();
        int n = seq.length;
        // 代表第一段连续红叶子、第二段连续黄叶子、第三段连续红叶子所需的最少替换数
        int rrr, ryy, ryr;
        // 初始化起始字符
        rrr = ryy = ryr = seq[0] == 'r' ? 0 : 1;
        // 初始化第二个字符（主要针对ryr的特殊情况）
        if (seq[1] == 'r') {
            ryy++;
            ryr++;
        } else {
            rrr++;
        } 
        for (int i = 2; i < n; i++) {
            if (seq[i] == 'r') {
                // ryr可由ryr与ryy拼接当前r得到
                ryr = Math.min(ryr, ryy);
                // ryy可由ryy、rrr拼接替换当前r为y得到
                ryy = Math.min(ryy + 1, rrr + 1);
            } else {
                // 替换y为r
                ryr = Math.min(ryr + 1, ryy + 1);
                // 拼接y
                ryy = Math.min(ryy, rrr);
                // 替换y为r
                rrr++;
            }
        }
        return ryr;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：16ms，在所有java提交中击败了97.78%的用户。

内存消耗：39.2MB，在所有java提交中击败了95.91%的用户。

&emsp;动态规划时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：14ms，在所有java提交中击败了99.40%的用户。

内存消耗：39.5MB，在所有java提交中击败了84.08%的用户。

## 官方解题

&emsp;暂无，密切关注。