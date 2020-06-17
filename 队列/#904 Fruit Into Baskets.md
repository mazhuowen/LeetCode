[toc]

In a row of trees, the `i`-th tree produces fruit with type `tree[i]`.

You **start at any tree of your choice**, then repeatedly perform the following steps:

* Add one piece of fruit from this tree to your baskets.  If you cannot, stop.
* Move to the next tree to the right of the current tree.  If there is no tree to the right, stop.

Note that you do not have any choice after the initial choice of starting tree: you must perform step 1, then step 2, then back to step 1, then step 2, and so on until you stop.

You have two baskets, and each basket can carry any quantity of fruit, but you want each basket to only carry one type of fruit each.

What is the total amount of fruit you can collect with this procedure?



**Note:**

* $1 \le \text{tree.length} \le 40000$
* $0 \le \text{tree[i]} < \text{tree.length}$



## 题目解读

&emsp;连续数字只能选择两种数值，求最大的连续长度。

```java
class Solution {
    public int totalFruit(int[] tree) {

    }
}
```

## 程序设计

* 使用双指针模拟队列，记录包含两个以下数值的队列长度。

```java
class Solution {
    public int totalFruit(int[] tree) {
        if (tree == null || tree.length == 0) return 0;

        // 计数
        Map<Integer, Integer> counter = new HashMap<>();
        // 队列指针
        int left = 0, right = 0;
        int res = 0;

        while (right < tree.length) {
            counter.put(tree[right], counter.getOrDefault(tree[right++], 0) + 1);

            while (counter.size() > 2) {
                counter.put(tree[left], counter.get(tree[left]) - 1);
                if (counter.get(tree[left]) == 0) counter.remove(tree[left]);
                left++;
            }

            res = Math.max(res, right - left);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：61ms，在所有java提交中击败了33.27%的用户。

内存消耗：48.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。