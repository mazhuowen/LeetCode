[toc]

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

word1 and word2 may be the same and they represent two individual words in the list.



**Note:**
You may assume *word1* and *word2* are both in the list.



## 题目解读

&emsp;不同于[#243 Shortest Word Distance](./#243 Shortest Word Distance.md)，给定的两个字符串可能相等。

```java
class Solution {
    public int shortestWordDistance(String[] words, String word1, String word2) {

    }
}
```

## 程序设计

* 使用两个变量保存字符串索引。需要注意两个字符串相等的情况，此时两个变量不能同时更新，否则距离总是0；当距离相等时，变量1保存变量2的值，变量2更新即可。

```java
class Solution {
    public int shortestWordDistance(String[] words, String word1, String word2) {
        if (words == null || words.length == 0) throw new IllegalArgumentException("invalid param");

        int distance = Integer.MAX_VALUE;
        boolean isSame = word1.equals(word2);
        Integer idx1 = null, idx2 = null;
        for (int i = 0; i < words.length; i++) {
            if (word1.equals(words[i])) {
                // 相等则idx1保存idx2的值
                if (isSame) idx1 = idx2 == null ? null : idx2;
                else idx1 = i;
            }
            if (word2.equals(words[i])) idx2 = i;

            if (idx1 != null && idx2 != null)
                distance = Math.min(distance, Math.abs(idx2 - idx1));
        }
        return distance == Integer.MAX_VALUE ? (isSame ? 0 : -1) : distance;
    }
}
```

## 性能分析

&emsp;世界爱你复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了22.77%的用户。

内存消耗：39.8MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;暂无，密切关注。