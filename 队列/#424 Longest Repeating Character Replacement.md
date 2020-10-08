[toc]


Given a string `s` that consists of only uppercase English letters, you can perform at most $k$ operations on that string.

In one operation, you can choose **any** character of the string and change it to any other uppercase English character.

Find the length of the longest sub-string containing all repeating letters you can get after performing the above operations.

**Note:**
Both the string's length and $k$ will not exceed $10^4$.



## 题目解读

&emsp;最多替换$k$次字符，求最长的单一字符构成的子字符串长度。

```java
class Solution {
    public int characterReplacement(String s, int k) {

    }
}
```

## 程序设计

* 题目可转换为在一段连续序列中不同的字符最对有$k$个，可见是滑动窗口问题。可统计窗口中相等字符最多的数目，然后不相等的字符最多只允许$k$个；
* 难点在于怎么动态统计窗口中的最大数目`max`，及左指针滑动时`max`怎么变化；实际不需要动态统计，只需增量统计即可，因为如果后续窗口中`max`比之前的小，则替换$k$个字符后形成的长度必然比之前短，故只有后面窗口`max`比之前大时才可判断滑动；
* 对于左指针的滑动，如果窗口中超过$k$个字符不同，则不管左侧是主体字符还是非主体字符，滑动至刚好$k$个为止，然后全部替换并更新最大长度。

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] counter = new int[26];
        int left = 0, right = 0;
        // 队列中最多的字符
        int max = 0, res = 0;
        while (right < s.length()) {
            char c = s.charAt(right);
            max = Math.max(max, ++counter[c - 'A']);
            // 不相等字符超过k个（保持窗口为max+k并不断扩大）
            while (right - left + 1 - max > k) {
                counter[s.charAt(left++) - 'A']--;
            }
            // max在不断扩大，得到的就是最大值
            res = right - left + 1;
            right++;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：5ms，在所有java提交中击败了92.86%的用户。

内存消耗：38.4MB，在所有java提交中击败了90.95%的用户。

## 官方解题

&emsp;暂无，密切关注。