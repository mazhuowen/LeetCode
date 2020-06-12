[toc]

Given a non-negative integer `N`, find the largest number that is less than or equal to `N` with monotone increasing digits.

(Recall that an integer has monotone increasing digits if and only if each pair of adjacent digits `x` and `y` satisfy $x \le y$.)



**Note:** 

`N` is an integer in the range $[0, 10^9]$.



## 题目解读

&emsp;找到不大于给定数字的最大递增数字。

```java
class Solution {
    public int monotoneIncreasingDigits(int N) {

    }
}
```

## 程序设计

* 观察规律，从头遍历数字，当遇到不满足递增条件的数时候，将当前数减一，其后的数字变为9；需要考虑到当前数字前面存在相等数字时，要定位到相同数字起始再做上述操作。

```java
class Solution {
    public int monotoneIncreasingDigits(int N) {
        if (N < 0) throw new IllegalArgumentException("invalid param");

        char[] strs = Integer.toString(N).toCharArray();
        // 查找不满足递增的分界线
        int idx = 0;
        while (idx < strs.length - 1 && strs[idx] <= strs[idx + 1]) idx++;
        // N满足条件，返回
        if (idx == strs.length - 1) return N;
        
        // idx位置减少一，需遍历相等的元素，定位到相等元素起始
        while (idx > 0 && strs[idx - 1] == strs[idx]) idx--;
        strs[idx]--;
        for (int i = idx + 1; i < strs.length; i++) {
            strs[i] = '9';
        }
        return Integer.valueOf(new String(strs));
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了98.65%的用户

内存消耗：36.4MB，在所有java提交中击败了33.33%的用户

## 官方解题

&emsp;同上。