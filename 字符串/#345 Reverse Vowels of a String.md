[toc]

Write a function that takes a string as input and reverse only the vowels of a string.



**Note:**
The vowels does not include the letter `"y"`.



## 题目解读

&emsp;翻转字符串中的元音字母对。

```java
class Solution {
    public String reverseVowels(String s) {

    }
}
```

## 程序设计

* 双指针遍历交换，需注意大小写。

```java
class Solution {
    public String reverseVowels(String s) {
        String seq = "aeiouAEIOU";
        char[] str = s.toCharArray();
        int left = 0, right = str.length - 1;
        while (left < right) {
            if (!seq.contains(str[left] + "")) left++;
            else if (!seq.contains(str[right] + "")) right--;
            else {
                char temp = str[left];
                str[left++] = str[right];
                str[right--] = temp;
            }
        }
        return new String(str);
    }
}
```

* 优化字符比较部分：

```java
class Solution {
    public String reverseVowels(String s) {
        String seq = "aeiouAEIOU";
        char[] str = s.toCharArray();
        int left = 0, right = str.length - 1;
        while (left < right) {
            if (!isVowels(str[left])) left++;
            else if (!isVowels(str[right])) right--;
            else {
                char temp = str[left];
                str[left++] = str[right];
                str[right--] = temp;
            }
        }
        return new String(str);
    }

    private boolean isVowels(char c) {
        switch(c) {
            case 'a':
            case 'e':
            case 'i':
            case 'o':
            case 'u':
            case 'A':
            case 'E':
            case 'I':
            case 'O':
            case 'U':
                return true;
            default:
                return false;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了96.63%的用户。

内存消耗：40.2MB，在所有java提交中击败了10.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路一致。