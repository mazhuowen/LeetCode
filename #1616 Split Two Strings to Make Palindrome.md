[toc]

You are given two strings `a` and `b` of the same length. Choose an index and split both strings **at the same index**, splitting `a` into two strings: `aprefix` and `asuffix` where `a = aprefix + asuffix`, and splitting `b` into two strings: `bprefix` and `bsuffix` where `b = bprefix + bsuffix`. Check if aprefix + bsuffix or bprefix + asuffix forms a palindrome.

When you split a string `s` into `sprefix` and `ssuffix`, either `ssuffix` or `sprefix` is allowed to be empty. For example, if `s = "abc"`, then `"" + "abc"`, `"a" + "bc"`, `"ab" + "c"` , and `"abc" + ""` are valid splits.

Return `true` if it is possible to form a palindrome string, otherwise return `false`.

**Notice** that `x + y` denotes the concatenation of strings `x` and `y`.



**Constraints**:

* $1 \le \text{a.length, b.length} \le 10^5$
* $\text{a.length} == \text{b.length}$
* `a` and `b` consist of lowercase English letters



## 题目解读

&emsp;判断在同一位置切分字符串后拼接是否形成回文。

```java
class Solution {
    public boolean checkPalindromeFormation(String a, String b) {
        
    }
}
```

## 程序设计

* 情况主要分四种，即`a`在前半部分占一半以上拼接后面的小部分`b`、`a`在后半部分占一半以上拼接前面的小部分`b`，及反之两种情况；
* 可用中心扩展来判断，以字符串`abccsd`为例，其前半部分为`abc`，由于其后的`c`与其前的`c`对称，只需判断另一个字符串最后两位是否是`ba`即可；
* 根据这个思路首先判断字符串`a`占大部分的情况，分为前、后拼接字符串`b`的子情况，可中心扩展得到最小拼接位置，然后继续中心扩展判断；然后判断`b`为大部分的情况。

```java
class Solution {
    public boolean checkPalindromeFormation(String a, String b) {
        if (a == null || b == null || a.length() != b.length()) throw new IllegalArgumentException("invalid param");
        char[] S = a.toCharArray(), T = b.toCharArray();
        return checkPalindrome(S, T) || checkPalindrome(T, S);
    }

    private boolean checkPalindrome(char[] S, char[] T) {
        int mid = S.length / 2 - 1;
        // 向左、向右拼接T是否可行
        boolean left = true, right = true;
        // 中心扩展寻找最小拼接点
        while (mid >= 0 && S[mid] == S[S.length - mid - 1]) mid--;
        // 向左向右判断
        while (mid >= 0 && (left || right)) {
            // 将T拼接到左侧不符合要求
            if (S[mid] != T[S.length - mid - 1]) right = false;
            // 将T拼接到右侧不符合要求
            if (T[mid] != S[S.length - mid - 1]) left = false;
            mid--;
        }
        // 判断是否有一个方向匹配完成
        return mid < 0 && (left || right);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;