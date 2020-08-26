[toc]

A string is called a happy prefix if is a **non-empty** prefix which is also a suffix (excluding itself).

Given a string `s`. Return the **longest happy prefix** of `s` .

Return an empty string if no such prefix exists.



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* `s` contains only lowercase English letters.



## 题目解读

&emsp;后缀最长匹配前缀问题。

```java
class Solution {
    public String longestPrefix(String s) {

    }
}
```

## 程序设计

* 可用`KMP`算法`next`数组解决。

```java
class Solution {
    public String longestPrefix(String s) {
        if (s == null || s.isEmpty()) throw new IllegalArgumentException("invalid param");

        int n = s.length(), k = -1;
        // KMP算法next数组
        int[] next = new int[n];
        next[0] = -1;
        for (int i = 1; i < n; i++) {
            while (k != -1 && s.charAt(k + 1) != s.charAt(i)) k = next[k];
            if (s.charAt(k + 1) == s.charAt(i)) k++;
            next[i] = k;
        }
        // 返回整个模式串的最大前缀
        return next[n - 1] == -1 ? "" : s.substring(0, next[n - 1] + 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：16ms，在所有java提交中击败了46.31%的用户。

内存消耗：40.7MB，在所有java提交中击败了26.19%的用户。

## 官方解题

&emsp;官方思路同上。