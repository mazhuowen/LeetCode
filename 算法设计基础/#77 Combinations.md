[toc]

Given two integers $n$ and $k$, return all possible combinations of $k$ numbers out of $1\dots n$.



## 题目解读

&emsp;找出所有的组合（注意不是排列）。

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {

    }
}
```

## 程序设计



```java
class Solution {
    List<List<Integer>> res;

    public List<List<Integer>> combine(int n, int k) {
        res = new LinkedList<>();

        combine(0, new ArrayList<>(), n, k);
        return res;
    }

    // start为起始数字，k为剩余数字
    private void combine(int start, List<Integer> list, int n, int k) {
        if (start > n) return;
        if (k == 0) {
            res.add(new ArrayList<>(list));
            return;
        }
        for (int i = start + 1; i <= n; i++) {

            // 尝试
            list.add(i);
            combine(i, list, n, k - 1);

            // 回溯
            list.remove(list.size() - 1);
        }
    }
}
```

* 上述算法可继续优化。在内层回溯尝试时，会遍历到最后一个数，这个循环实际上可以提前结束，因为当还剩$k$个数时，遍历到$n - k + 1$之后的数字组成的序列不满足元素数量要求，剪枝得：

```java
class Solution {
    List<List<Integer>> res;

    public List<List<Integer>> combine(int n, int k) {
        res = new LinkedList<>();

        combine(0, new ArrayList<>(), n, k);
        return res;
    }

    private void combine(int start, List<Integer> list, int n, int k) {
        if (start > n) return;
        if (k == 0) {
            res.add(new ArrayList<>(list));
            return;
        }
        for (int i = start + 1; i <= n - k + 1; i++) {

            // 尝试
            list.add(i);
            combine(i, list, n, k - 1);

            // 回溯
            list.remove(list.size() - 1);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(C_N^K)$。

执行用时：23ms，在所有java提交中击败了55.44%的用户。

内存消耗：41.3MB，在所有java提交中击败了51.85%的用户。

&emsp;优化后时间复杂度为$O(C_N^K)$，空间复杂度为$O(C_N^K)$。

执行用时：2ms，在所有java提交中击败了99.36%的用户。

内存消耗：41.3MB，在所有java提交中击败了51.85%的用户。

## 官方解题

&emsp;官方还提供了字典序的方法。