[toc]

Consider the string `s` to be the infinite wraparound string of "abcdefghijklmnopqrstuvwxyz", so `s` will look like this: "...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd....".

Now we have another string `p`. Your job is to find out how many unique non-empty substrings of `p` are present in `s`. In particular, your input is the string `p` and you need to output the number of different non-empty substrings of `p` in the string `s`.

**Note**: `p` consists of only lowercase English letters and the size of `p` might be over $10000$.



## 题目解读

&emsp;字符串`s`是`a~z`的循环字符串，给定字符串`p`，求`p`中子字符串在`s`中出现的数目。

```java
class Solution {
    public int findSubstringInWraproundString(String p) {
        
    }
}
```

## 程序设计

* 最基本的，可从头开始统计判断是否连续，但该方法存在一个问题，即无法判断重复，如当前位置是`abc`，后面某个位置出现`ab`会重复计数；
* 转变思路，由于`s`中的字符串组合是固定的，如果知道首字符或尾字符，就可以唯一确定子字符串，故可统计以该首字符起始（或尾字符结尾）的最长长度，这样以该首字符起始或尾字符结尾的子字符串组合数就是长度数。

```java
class Solution {
    public int findSubstringInWraproundString(String p) {
        if (p == null || p.isEmpty()) return 0;

        char[] strs = p.toCharArray();
        // 当前字符起始的最长字符串长度
        int[] max = new int[26];
        // 字符串结尾字符索引
        int pre = strs.length - 1;
        max[strs[pre] - 'a'] = 1;
        for (int i = strs.length - 2; i >= 0; i--) {
            char c = strs[i];
            // 连续
            if (c == strs[i + 1] - 1 || c == 'z' && strs[i + 1] == 'a') {
                max[c - 'a'] = Math.max(max[c - 'a'], pre - i + 1);
            } 
            // 不连续
            else {
                pre = i;
                if (max[c - 'a'] < 1) max[c - 'a'] = 1;
            }
        }

        int res = 0;
        for (int count : max) res += count;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了92.72%的用户。

内存消耗：39.9MB，在所有java提交中击败了32.43%的用户。

## 官方解题

&emsp;暂无，密切关注。