[toc]

Given a string `S`, return the number of substrings that have only **one distinct** letter.



**Constraints**:

* $1 \le \text{S.length} \le 1000$
* `S[i]` consists of only lowercase English letters.



## 题目解读

&emsp;统计只有一种字符的子字符串数目。

```java
class Solution {
    public int countLetters(String S) {
        
    }
}
```

## 程序设计

* 采用滑动窗口统计相同字符构成的最长子数组，设为$l$，则这段子数组中可构成的字符串数目为$1 + 2 + \dots + l = \frac{l * (l + 1)}{2}$。

```java
class Solution {
    public int countLetters(String S) {
        if (S == null || S.isEmpty()) return 0;

        char[] seqs = S.toCharArray();
        int res = 0, pre = 0;
        for (int i = 1; i < seqs.length; i++) {
            // 计算子字符串组合
            if (seqs[i - 1] != seqs[i]) {
                int l = i - pre;
                res += l * (1 + l) / 2;
                pre = i;
            }
        }
        // 最后一个子字符串
        int l = seqs.length - pre;
        res += l * (1 + l) / 2;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.3MB，在所有java提交中击败了65.79%的用户。

## 官方解题

&emsp;同上。