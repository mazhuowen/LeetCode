[toc]

Given an input string , reverse the string word by word. 

Example:

```
Input:  ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
```

Note: 

* A word is defined as a sequence of non-space characters.
* The input string does not contain leading or trailing spaces.
* The words are always separated by a single space.

Follow up: Could you do it in-place without allocating extra space?



## 题目解读

&emsp;翻转单词。

```java
class Solution {
    public void reverseWords(char[] s) {
        
    }
}
```

## 程序设计

* 首先翻转每个单词，然后翻转整个句子。

```java
class Solution {
    public void reverseWords(char[] s) {
        int i = 0;
        // 翻转每个单词
        for (int j = 0; j < s.length; j++) {
            if (s[j] == ' ') {
                reverse(s, i, j - 1);
                i = j + 1;
            }
        }
        // 最后一个单词
        reverse(s, i, s.length - 1);
        // 翻转句子
        reverse(s, 0, s.length - 1);
    }

    // 翻转函数
    private void reverse(char[] s, int i, int j) {
        while (i < j) {
            char temp = s[i];
            s[i++] = s[j];
            s[j--] = temp;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：47.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。