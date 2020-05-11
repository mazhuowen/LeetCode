[toc]

Given a string `S`, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.



**Note:**

1. $\text{S.length} \le 100$
2. $33 \le \text{S[i].ASCIIcode} \le 122$ 
3. `S` doesn't contain `\` or `"`



## 题目解读

&emsp;交换字符串中字母的位置，非字母位置不变。

```java
class Solution {
    public String reverseOnlyLetters(String S) {

    }
}
```

## 程序设计

* 数组交换。

```java
class Solution {
    public String reverseOnlyLetters(String S) {
        if (S == null || S.isEmpty()) return S;

        char[] str = S.toCharArray();
        int left = 0, right = str.length - 1;
        while (left < right) {
            while (left < right && !Character.isLetter(str[left])) left++;
            while (left < right && !Character.isLetter(str[right])) right--;
            // 定位字母并交换
            if (left < right) swap(str, left++, right--);
        }
        return new String(str);
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

执行用时：1ms，在所有java提交中击败了82.38%的用户。

内存消耗：37.6MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;官方思路类似。