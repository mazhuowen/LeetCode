[toc]

A string `S` of lowercase English letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.



**Note**:

* `S` will have length in range $[1, 500]$.
* `S` will consist of lowercase English letters (`'a'` to `'z'`) only.



## 题目解读

&emsp;求切分字符串使得每部分的字符都只在当前部分出现，得出每部分的长度。

```java
class Solution {
    public List<Integer> partitionLabels(String S) {

    }
}
```

## 程序设计

* 统计字母出现的最后一个位置，然后合并区间即可。

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> res = new LinkedList<>();
        if (S == null || S.isEmpty()) return res;
        int[] index = new int[26];
        char[] seq = S.toCharArray();
        // 记录最后出现的索引
        for (int i = 0; i < seq.length; i++) index[seq[i] - 'a'] = i;
        // 扩展界限
        int left = 0, right = index[seq[0] - 'a'];
        for (int i = 1; i < seq.length; i++) {
            // 新区见
            if (i > right) {
                res.add(right - left + 1);
                left = i;
                right = index[seq[i] - 'a'];
            } else {
                right = Math.max(right, index[seq[i] - 'a']);
            }
        }
        res.add(seq.length - left);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了97.05%的用户。

内存消耗：37.6MB，在所有java提交中击败了47.25%的用户。

## 官方解题

&emsp;官方思路同上。