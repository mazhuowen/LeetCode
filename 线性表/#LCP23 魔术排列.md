[toc]

秋日市集上，魔术师邀请小扣与他互动。魔术师的道具为分别写有数字 $1 \sim N$ 的 $N$ 张卡牌，然后请小扣思考一个 $N$ 张卡牌的排列 `target`。

魔术师的目标是找到一个数字 $k$（$k \ge 1$），使得初始排列顺序为 $1 \sim N$ 的卡牌经过特殊的洗牌方式最终变成小扣所想的排列 `target`，特殊的洗牌方式为：

* 第一步，魔术师将当前位于 **偶数位置** 的卡牌（下标自 $1$ 开始），保持 **当前排列顺序** 放在位于 **奇数位置** 的卡牌之前。例如：将当前排列 $[1,2,3,4,5]$ 位于偶数位置的 $[2,4]$ 置于奇数位置的 $[1,3,5]$ 前，排列变为 $[2,4,1,3,5]$；
* 第二步，若当前卡牌数量小于等于 $k$，则魔术师按排列顺序取走全部卡牌；若当前卡牌数量大于 $k$，则取走前 $k$ 张卡牌，剩余卡牌继续重复这两个步骤，直至所有卡牌全部被取走；

卡牌按照魔术师取走顺序构成的新排列为「魔术取数排列」，请返回是否存在这个数字 $k$ 使得「魔术取数排列」恰好就是 `target`，从而让小扣感到大吃一惊。



**提示：**

- $1 \le \text{target.length} = N \le 5000$
- 题目保证 `target` 是 $1 \sim N$ 的一个排列。



## 题目解读

&emsp;给定洗牌方式，求是否可得到目标排列。

```java
class Solution {
    public boolean isMagic(int[] target) {

    }
}
```

## 程序设计

* 题目判断存在某个$k$，使得根据规则洗牌后得到目标排列，由于每个步骤都需要把偶数位置的数字提到数组前面，然后取前$k$个，这就意味这我们可以根据目标排列得到数值$k$；得到$k$后只需模拟判断即可，即对剩余的数组判断变换后与目标序列相等的最大的$curK$值，除了最后一段序列，其他段$curK \ge k$，否则不满足条件；
* 如`[2,4,6,3,7,1,5]`，通过`2,4,6`得到$k = 3$，而第二段最长相等序列为`3,7,1,5`，得到的$curK = 4$，截取序列为$3$的部分即可，最后判断最后一段`5`，得到$curK = 1$，由于是最后一段，满足条件。

```java
class Solution {
    public boolean isMagic(int[] target) {
        int n = target.length;
        // 初始化卡牌
        int[] cards = new int[n];
        for (int i = 0; i < n; i++) cards[i] = i + 1;
        // 模拟
        return shuffleCards(cards, target, 0, -1);
    }

    private boolean shuffleCards(int[] cards, int[] target, int idx, int k) {
        if (idx >= cards.length) return true;
        // 模拟本轮，将偶数位置提前
        int[] tmp = Arrays.copyOf(cards, cards.length);
        int insert = idx;
        for (int j = idx + 1; j < cards.length; j += 2) cards[insert++] = tmp[j];
        for (int j = idx; j < cards.length; j += 2) cards[insert++] = tmp[j];
        

        // 根据前面获取k
        int curK = 0;
        for (int i = idx; i < cards.length; i++) {
            if (target[i] != cards[i]) break;
            curK++;
        }
        // 第一次计算k，赋值
        if (k == -1) k = curK;
        // 当前的k在不是最后一段序列的情况下小于k，则说明不存在
        if (curK == 0 || idx + curK < cards.length && curK < k) return false;
        
        return shuffleCards(cards, target, idx + k, k);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.3MB，在所有java提交中击败了36.36%的用户。

## 官方解题

&emsp;暂无，密切关注。