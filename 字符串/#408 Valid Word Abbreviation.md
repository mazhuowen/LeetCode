[toc]

Given a **non-empty** string `s` and an abbreviation `abbr`, return whether the string matches with the given abbreviation.

A string such as `"word"` contains only the following valid abbreviations:

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```


Notice that only the above abbreviations are valid abbreviations of the string `"word"`. Any other string is not a valid abbreviation of `"word"`.

Note:
Assume s contains only lowercase letters and abbr contains only lowercase letters and digits.



## 题目解读

&emsp;检测缩写与单词是否一致。

```java
class Solution {
    public boolean validWordAbbreviation(String word, String abbr) {

    }
}
```

## 程序设计

* 题目简单，遇到字母则比较字母，遇到数字则跳过这段数字长度的序列继续比较，但是边角情况很多，需要仔细判断。首先`abbr`可能是非法的，比如存在单独的0或以0开头的情况，需要判断排除；其次最后两个序列必须同时遍历结束，不能存在一个遍历完另一个还剩一段的情况。

```java
class Solution {
    public boolean validWordAbbreviation(String word, String abbr) {
        if (word.isEmpty()) return abbr.isEmpty();

        char[] words = word.toCharArray();
        char[] abbrs = abbr.toCharArray();

        int wIdx = 0, aIdx = 0;
        while (wIdx < words.length) {
            // 记录当前位置，便于判断数字是否合法
            int temp = aIdx;
            // 如果是数字，则计算数字
            int num = 0;
            while (aIdx < abbrs.length && Character.isDigit(abbrs[aIdx])) {
                num = num *10 + (abbrs[aIdx] - '0');
                // 存在数字0开头的数字，不合法
                if (aIdx > temp && num == (abbrs[aIdx] - '0')) return false;
                aIdx++;
            }
            
            // 数字为0，要么当前是字母不是数字，要么是数字0，需要进一步判断
            if (num == 0) {
                // temp + 1 == aId表示只有一位数字0，不合法。
                // aIdx >= abbrs.length表示abbr已遍历完，不符合要求；或者字母不匹配，不符合要求
                if (temp + 1 == aIdx ||  aIdx >= abbrs.length || words[wIdx++] != abbrs[aIdx++]) return false;
            } else {
                // 跳到num个字母后继续比较
                wIdx += num;
            }
        }
        // 最后必须都是末尾，避免"a"，"2"的情况判断为正确，或者abbr还未遍历完
        return wIdx == words.length && aIdx == abbrs.length;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB, 在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;思路同上。