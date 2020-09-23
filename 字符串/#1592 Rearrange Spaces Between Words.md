[toc]

You are given a string `text` of words that are placed among some number of spaces. Each word consists of one or more lowercase English letters and are separated by at least one space. It's guaranteed that `text` **contains at least one word**.

Rearrange the spaces so that there is an **equal** number of spaces between every pair of adjacent words and that number is **maximized**. If you cannot redistribute all the spaces equally, place the **extra spaces at the end**, meaning the returned string should be the same length as `text`.

Return the string after rearranging the spaces.



**Constraints**:

* $1 \le \text{text.length} \le 100$
* `text` consists of lowercase English letters and `' '`.
* `text` contains at least one word.



## 题目解读

&emsp;给定文本，其中含有空格，将空格重新排序，使得每个单词间空格数目相等。

```java
class Solution {
    public String reorderSpaces(String text) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public String reorderSpaces(String text) {
        if (text == null || text.isEmpty()) return text;
        
        // 拆分字符串，统计单词长度
        String[] words = text.split(" ");
        int w = 0, len = 0;
        for (String word : words) {
            if (word.isEmpty()) continue;
            w++;
            len += word.length();
        }
        
        // 计算平摊的空格数目
        if (w > 1) len = (text.length() - len) / (w - 1);

        String sp = prodSpace(len);
        StringBuffer res = new StringBuffer();
        // 拼接单词和空格
        for (String word : words) {
            if (word.isEmpty()) continue;
            res.append(word);
            res.append(sp);
        }
        
        // 除去多余的空格
        if (res.length() > text.length()) return res.toString().substring(0, text.length());
        // 补齐空格
        else {
            res.append(prodSpace(text.length() - res.length()));
            return res.toString();
        }
    }
    
    // 产生空格
    private String prodSpace(int n) {
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < n; i++) sb.append(" ");
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了67.14%的用户。

内存消耗：37.4MB，在所有java提交中击败了19.88%的用户。

## 官方解题

&emsp;暂无，密切关注。