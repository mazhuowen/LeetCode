[toc]

A group of friends went on holiday and sometimes lent each other money. For example, Alice paid for Bill's lunch for \$10. Then later Chris gave Alice ​\$5 for a taxi ride. We can model each transaction as a tuple `(x, y, z)` which means person `x` gave person `y` ​`$z`. Assuming Alice, Bill, and Chris are person 0, 1, and 2 respectively (0, 1, 2 are the person's ID), the transactions can be represented as `[[0, 1, 10], [2, 0, 5]]`.

Given a list of transactions between a group of people, return the minimum number of transactions required to settle the debt.



**Note**:

* A transaction will be given as a tuple `(x, y, z)`. Note that $x \ne y$ and $z > 0$.
* Person's IDs may not be linear, e.g. we could have the persons 0, 1, 2 or we could also have the persons 0, 2, 6.



## 题目解读

&emsp;给定交易列表，求完成债务的最少次数。

```java
class Solution {
    public int minTransfers(int[][] transactions) {

    }
}
```

## 程序设计

* 参考社区思路，本题为`NP`问题，采用回溯遍历计算。首先根据借钱和还债统计出每个人的财产情况，问题变为尝试最少的次数使得正负财产相抵消。

```java
class Solution {
    int min = Integer.MAX_VALUE;

    public int minTransfers(int[][] transactions) {
        if (transactions == null || transactions.length == 0) return 0;

        Map<Integer, Integer> record = new HashMap<>();
        for (int[] transaction : transactions) {
            int from = transaction[0], to = transaction[1], val = transaction[2];
            record.put(from, record.getOrDefault(from, 0) - val);
            record.put(to, record.getOrDefault(to, 0) + val);
        }

        // 转化为数组方便遍历
        Integer[] money = record.values().toArray(new Integer[0]);
        dfs(money, 0, 0);
        return min;
    }

    private void dfs(Integer[] money, int idx, int count) {
        // 剪枝
        if (count >= min) return;

        while (idx < money.length && money[idx] == 0) idx++;
        if (idx >= money.length) {
            min = Math.min(min, count);
            return;
        }

        for (int i = idx + 1; i < money.length; i++) {
            if (money[idx] > 0 && money[i] < 0 || money[idx] < 0 && money[i] > 0) {
                // 尝试将idx的钱或债放入i
                money[i] += money[idx];
                dfs(money, idx + 1, count + 1);
                money[i] -= money[idx];
            }
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了80.26%的用户。

内存消耗：37.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。