[toc]

Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.



Note: In the string, each word is separated by single space and there will not be any extra space in the string.



## 题目解读

&emsp;翻转句子中的单词。单词用空格表示

```java
class Solution {
    public String reverseWords(String s) {

    }
}
```

## 程序设计

* 注意题目限定单词之间只有一个空格，没有多余的空格。字符交换位置翻转。

```java
class Solution {
    public String reverseWords(String s) {
        if (s == null || s.isEmpty()) return s;

        char[] strs = s.toCharArray();
        int start = 0, end = 0;
        while (end < strs.length) {
            // 翻转前一个单词
            if (strs[end] == ' ') {
                reverse(strs, start, end - 1);
                // 更新起始位置
                start = end + 1;
            }
            end++;
        }
		// 最后一个单词
        reverse(strs, start, end - 1);

        return new String(strs);
    }

    private void reverse(char[] str, int start, int end) {
        while (start < end) {
            swap(str, start++, end--);
        }
    }

    private void swap(char[] str, int i, int j) {
        char temp = str[i];
        str[i] = str[j];
        str[j] = temp;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了99.73%的用户。

内存消耗：39.8MB，在所有java提交中击败了10.00%的用户。

## 官方解题

&emsp;还提供了调用`API`方式的思路。