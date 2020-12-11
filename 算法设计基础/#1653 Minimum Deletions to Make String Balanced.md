[toc]

You are given a string `s` consisting only of characters `'a'` and `'b'`.

You can delete any number of characters in `s` to make `s` **balanced**. `s` is **balanced** if there is no pair of indices `(i,j)` such that $i < j$ and `s[i] = 'b'` and `s[j] = 'a'`.

Return the **minimum** number of deletions needed to make `s` **balanced**.

 

**Example 1**:

```
Input: s = "aababbab"
Output: 2
Explanation: You can either:
Delete the characters at 0-indexed positions 2 and 6 ("aababbab" -> "aaabbb"), or
Delete the characters at 0-indexed positions 3 and 6 ("aababbab" -> "aabbbb").
```

**Example 2**:

```
Input: s = "bbaaaaabb"
Output: 2
Explanation: The only solution is to delete the first two characters.
```



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* `s[i]` is `'a'` or `'b'`.



## 题目解读

&emsp;删除字符使得剩余的字符为`aa…bb`，所有的`a`在`b`前面，求最少的删除数。

```java
class Solution {
    public int minimumDeletions(String s) {

    }
}
```

## 程序设计

* 符合条件的字符串要么全部是`a`，要么全部是`b`，要么是`aa…bb`，则可定义状态`aa`表示全是`a`的删除数，状态`bb`表示全是`b`的删除数，`ab`表示前面全是`a`，后面全是`b`的修改数。

```java
class Solution {
    public int minimumDeletions(String s) {
        int aa = 0, bb = 0, ab = Integer.MAX_VALUE;
        int idx = 0;
        char[] seq = s.toCharArray();
        while (idx < seq.length && seq[idx] == 'b') {
            aa++;
            idx++;
        }
        while (idx < seq.length && seq[idx] == 'a') {
            bb++;
            idx++;
        }
        // 初始化ab
        if (idx++ < seq.length) {
            ab = aa++;
        }
        // 动态规划
        while (idx < seq.length) {
            if (seq[idx++] == 'a') {
                // 删除当前a
                ab++;
                bb++;
            } else {
                // ab选择之前aa与当前b或之前ab删除当前b两种方法中的最小值
                ab = Math.min(aa, ab);
                aa++;
            }
        }
        // 最小值
        return Math.min(aa, Math.min(bb, ab));
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：25 ms, 在所有 Java 提交中击败了96.46%的用户

内存消耗：39.2 MB, 在所有 Java 提交中击败了78.46%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有采用数组前后首尾遍历两次统计全是`a`和全是`b`的删除数目，然后再次统计计算之前是`a`之后是`b`的最小删除数目。