[toc]

某互联网公司一年一度的春招开始了，一共有 $n$ 名面试者入选。每名面试者都会提交一份简历，公司会根据提供的简历资料产生一个预估的能力值，数值越大代表越有可能通过面试。

小A 和小B 负责审核面试者，他们均有所有面试者的简历，并且将各自根据面试者能力值从大到小的顺序浏览。由于简历事先被打乱过，能力值相同的简历的出现顺序是从它们的全排列中等可能地取一个。现在给定 $n$ 名面试者的能力值 `scores`，设 `X` 代表小A 和小B 的浏览顺序中出现在同一位置的简历数，求 `X` 的期望。

提示：离散的非负随机变量的期望计算公式为 $E(X) = \sum_{k = 1}^\infty kPr(X = k)$。在本题中，由于 `X` 的取值为 $0$ 到 $n$ 之间，期望计算公式可以是 $E(X) = \sum_{k = 1}^nkPr(X = k)$。



**限制：**

- $1 \le \text{scores.length} \le 10^5$
- $0 \le \text{scores[i]} \le 10^6$



## 题目解读

&emsp;计算两个面试者在同一位置看到同一简历的期望。注意按照能力排序，能力值相等的人简历是打乱的。

```java
class Solution {
    public int expectNumber(int[] scores) {

    }
}
```

## 程序设计

* 假设分数相等的候选人有$x$个，则当前位置可能有$x$个候选人，面试官正好都是$x$的简历的概率为$\frac{1}{x^2}$，则当前位置期望为$\frac{1}{x}$，$x$个相等位置期望为$1$，即最后问题转化为数组中有多少个不相等数；
* 可数组遍历判断。

```java
class Solution {
    public int expectNumber(int[] scores) {
        Arrays.sort(scores);
        int ex = 0, n = scores.length;
        int pre = -1;
        for (int i = 0; i < n; i++) {
            if (pre == -1 || scores[i] != scores[pre]) {
                // 计算期望
                if (pre != -1) ex += 1;
                pre = i;
            }
        }
        // 最后一段
        ex += 1;
        return ex;
    }
}
```

* 由于是不同数字统计，可引入哈希表，简化时间复杂度。

```java
class Solution {
    public int expectNumber(int[] scores) {
        Set<Integer> set = new HashSet();
        for (int score : scores) set.add(score);
        return set.size();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：29ms，在所有java提交中击败了29.44%的用户。

内存消耗：47.2MB，在所有java提交中击败了96.89%的用户。

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：21ms，在所有java提交中击败了91.24%的用户。

内存消耗：47.8MB，在所有java提交中击败了51.36%的用户。

## 官方解题

&emsp;同上。