[toc]

小扣在秋日市集选择了一家早餐摊位，一维整型数组`staple`中记录了每种主食的价格，一维整型数组`drinks`中记录了每种饮料的价格。小扣的计划选择一份主食和一款饮料，且花费不超过`x`元。请返回小扣共有多少种购买方案。

注意：答案需要以 $1e9 + 7$ ($1000000007$) 为底取模，如：计算初始结果为：$1000000008$，请返回 $1$



提示：

* $1 \le \text{staple.length} \le 10^5$
* $1 \le \text{drinks.length} \le 10^5$
* $1 \le \text{staple[i],drinks[i]} \le 10^5$

* $1 \le x \le 2*10^5$



## 题目解读

&emsp;计算主食与饮料组合。

```java
class Solution {
    public int breakfastNumber(int[] staple, int[] drinks, int x) {
        
    }
}
```

## 程序设计

* 可排序然后使用双指针遍历实现。

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int breakfastNumber(int[] staple, int[] drinks, int x) {
        Arrays.sort(staple);
        Arrays.sort(drinks);
        int res = 0;
        int left = 0, right = drinks.length - 1;
        while (left < staple.length) {
            while (right >= 0 && staple[left] + drinks[right] > x) right--;
            res = (res + right + 1) % MOD;
            left++;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(N\log_2N,M\log_2M,N + M))$，空间复杂度为$O(1)$。

执行用时：86ms，在所有java提交中击败了100.00%的用户。

内存消耗：59.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。