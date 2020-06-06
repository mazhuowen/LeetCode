[toc]

Given a string `s`. Return all the words vertically in the same order in which they appear in `s`.
Words are returned as a list of strings, complete with spaces when is necessary. (Trailing spaces are not allowed).
Each word would be put on only one column and that in one column there will be only one word.



Constraints:

* $1 \le \text{s.length} \le 200$
* `s` contains only upper case English letters.
* It's guaranteed that there is only one space between 2 words.



## 题目解读

&emsp;给定句子字符串。按照单词在句子中的出现顺序全部竖直返回。单词应该以字符串列表的形式返回，必要时用空格补位，但输出尾部的空格需要删除（不允许尾随空格）。
每个单词只能放在一列上，每一列中也只能有一个单词。

```java
class Solution {
    public List<String> printVertically(String s) {
    
    }
}
```

## 程序设计

* 模拟遍历每列并加入对应列的字符串。需要注意当前字符在当前列的位置必须与所在单词在句子中的顺序相同，如果前面不连续则需要拼接空格。

```java
class Solution {
    public List<String> printVertically(String s) {
        List<String> res = new LinkedList<>();
        if (s == null || s.isEmpty()) return res;

        String[] strs = s.split(" ");
        List<StringBuffer> temp = new LinkedList<>();
        for (int i = 0; i < strs.length; i++) {
            String str = strs[i];
            char[] letters = str.toCharArray();
            for (int j = 0; j < letters.length; j++) {
                if (j == temp.size()) temp.add(new StringBuffer());
                StringBuffer sb = temp.get(j);
                // 如果前面不连续，填充空格
                while (sb.length() < i) {
                    sb.append(" ");
                }
                // 填充当前字符
                temp.get(j).append(letters[j]);
            }
        }

        for (StringBuffer sb : temp) res.add(sb.toString());
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$，其中$M$为单词最长长度。

执行用时：1ms，在所有java提交中击败了99.25%的用户。

内存消耗：37.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。