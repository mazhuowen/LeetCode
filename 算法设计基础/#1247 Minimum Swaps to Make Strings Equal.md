[toc]

You are given two strings `s1` and `s2` of equal length consisting of letters `"x"` and `"y"` **only**. Your task is to make these two strings equal to each other. You can swap any two characters that belong to **different** strings, which means: swap `s1[i]` and `s2[j]`.

Return the minimum number of swaps required to make `s1` and `s2` equal, or return `-1` if it is impossible to do so.



**Constraints:**

- $1 \le \text{s1.length, s2.length} \le 1000$
- `s1, s2` only contain `'x'` or `'y'`.



## 题目解读

&emsp;交换两个字符串中的字符，使得字符串相等。题目规定字符串只包含`x`和`y`。

```java
class Solution {
    public int minimumSwap(String s1, String s2) {

    }
}
```

## 程序设计

* 利用贪婪法的思路，当当前字符相等时，不用交换，只需交换不匹配的字符对；这样我们只需考虑不匹配的字符对，由字符串1和字符串2构成的不匹配字符对分别为`x-y`，`y-x`；此处涉及到两种交换模式：
  * 两个相同不匹配字符对交换：以两个`x-y`为例，只需交换第一个的`x`和第二个的`y`或第一个的`y`和第二个的`x`就可得到两个匹配串，只需一次交换；
  * 两个不同不匹配字符对交换：即`x-y`与`y-x`的情况，首先交换第一个`x`和第二个的`y`或第一个的`y`和第二个的`x`，得到情况1，然后再次交换，总共需要两次交换。
* 统计两种不匹配对的数目，如果不匹配对的总数目是奇数，显然无法交换相等；由上述分析，使得尽可能多的不匹配对做情况一的交换，只需交换一次，若有剩余则做情况二的交换。
* 假设两种不匹配对的数目为`xy`和`yx`，若都是偶数，则交换次数为$xy / 2 + yx / 2$；如果都是奇数，则尽可能多的交换偶数个，剩余的两个不匹配对做情况二的交换，需要交换两次，数目为$(xy - 1)/2 + (yx - 1)/2 + 2$。计算机除法合并得$(xy + 1)/2 + (yx + 1)/2$。

```java
class Solution {
    public int minimumSwap(String s1, String s2) {
        char[] seq1 = s1.toCharArray(), seq2 = s2.toCharArray();

        int xy = 0, yx = 0;
        for (int i = 0; i < seq1.length; i++) {
            if (seq1[i] == seq2[i]) continue;
            // 不相等，需要交换
            if (seq1[i] == 'x') xy++;
            else yx++;
        }

        // 交换对必须是奇数个
        if ((xy + yx) % 2 == 1) return -1;
        return (xy + 1) / 2 + (yx + 1) / 2;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.9MB，在所有java提交中击败了12.09%的用户。

## 官方解题

&emsp;暂无，密切关注。