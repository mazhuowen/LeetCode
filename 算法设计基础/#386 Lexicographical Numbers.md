[toc]

Given an integer $n$, return $1 \sim n$ in lexicographical order.

For example, given 13, return: `[1,10,11,12,13,2,3,4,5,6,7,8,9]`.

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.



## 题目解读

&emsp;返回$1 \sim n$的字典序。

```java
class Solution {
    public List<Integer> lexicalOrder(int n) {

    }
}
```

## 程序设计

* 回溯遍历拼接。

```java
class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> res = new LinkedList<>();
        if (n < 1) return res;

        for (int i = 1; i <= 9; i++) {
            lexicalOrder(i, res, n);
        }
        return res;
    }

    private void lexicalOrder(int preNum, List<Integer> res, int limit) {
        if (preNum > limit) return;
		// 拼接，由于先遍历小的值，故拼接后顺序就是字典序
        res.add(preNum);
        // 回溯
        for (int i = 0; i <= 9; i++) {
            lexicalOrder(preNum * 10 + i, res, limit);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(9^d)$，空间复杂度为$O(d)$。

执行用时：6ms，在所有java提交中击败了57.14%的用户。

内存消耗：46.8MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;暂无，密切关注。