[toc]

Given a column title as appear in an Excel sheet, return its corresponding column number.



## 题目解读

&emsp;将`Excel`行标签的字母转化为数字。

```java
class Solution {
    public int titleToNumber(String s) {
        
    }
}
```

## 程序设计

* 仔细观察会发现字母表示的数字是26进制数。

```java
class Solution {
    public int titleToNumber(String s) {
        if (s == null || s.length() == 0) throw new IllegalArgumentException("invalid param");

        int res = 0;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            // 26进制拼接，需注意没有0，要加一
            res = res * 26 + (s.charAt(i) - 'A' + 1);
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.3MB，在所有java提交中击败了5.55%的用户。

## 官方解题

&emsp;暂无，密切关注。