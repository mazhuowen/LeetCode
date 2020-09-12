[toc]

小扣在秋日市集发现了一款速算机器人。店家对机器人说出两个数字（记作 `x` 和 `y`），请小扣说出计算指令：

* `"A"` 运算：使 $x = 2 * x + y$；
* `"B"` 运算：使 $y = 2 * y + x$。

在本次游戏中，店家说出的数字为 $x = 1$ 和 $y = 0$，小扣说出的计算指令记作仅由大写字母 `A`、`B` 组成的字符串 `s`，字符串中字符的顺序表示计算顺序，请返回最终 `x` 与 `y` 的和为多少。

**提示：**

- $0 \le \text{s.length} \le 10$
- `s` 由 `'A'` 和 `'B'` 组成



## 题目解读

&emsp;遍历计算。

```java
class Solution {
    public int calculate(String s) {

    }
}
```

## 程序设计

* 遍历计算。

```java
class Solution {
    public int calculate(String s) {
        int x = 1, y = 0;
        for (char c : s.toCharArray()) {
            if (c == 'A') x = 2 * x + y;
            else y = 2 * y + x;
        }
        return x + y;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。