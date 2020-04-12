[toc]

The count-and-say sequence is the sequence of integers with the first five terms as following:

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2, then one 1"` or `1211`.

Given an integer $n$ where $1 \le n \le 30$, generate the $n^{th}$ term of the count-and-say sequence. You can do so recursively, in other words from the previous member read off the digits, counting the number of digits in groups of the same digit.

Note: Each term of the sequence of integers will be represented as a string.



## 题目解读

&emsp;外观数列是一个整数，从数字1开始，序列中的每一项是对前一项的描述。给定$n$，求该序列。

```java
class Solution {
    public String countAndSay(int n) {

    }
}
```

## 程序设计

* 外观序列每一个当前序列是对前一个序列的计数统计拼接而成，有了这个规律，可以使用递归实现。

```java
class Solution {
    public String countAndSay(int n) {
        if (n <= 0) return "";
        // 从1开始
        return countAndSay("1", n - 1);
    }

    private String countAndSay(String source, int n) {
        if (n == 0) return source;
        StringBuffer sb = new StringBuffer();
        // 当前数字及其计数
        Integer pre = null, count = 0;
        for (char c : source.toCharArray()) {
            // 相等则计数
            if (pre != null && c - '0' == pre) count++;
            else {
                // 当前数字与之前数字不同，拼接之前数字
                if (pre != null) sb.append(count).append(pre);
                // 重置当前数字及其计数
                pre = c - '0';
                count = 1;
            }
        }
        // 最后拼接末尾数字
        sb.append(count).append(pre);
        // 递归
        return countAndSay(sb.toString(), n - 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，尾递归空间复杂度为$O(M)$，其中$N$为参数，$M$为字符串长度。

执行用时：2ms，在所有java提交中击败了84.31%的用户。

内存消耗：38.8MB，在所有java提交中击败了6.11%的用户。

## 官方解题

&emsp;暂无，密切关注。社区方法思路类似。