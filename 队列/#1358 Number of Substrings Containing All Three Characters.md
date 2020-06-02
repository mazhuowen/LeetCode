[toc]

Given a string `s` consisting only of characters `a`, `b` and `c`.

Return the number of substrings containing **at least** one occurrence of all these characters `a`, `b` and `c`.



Constraints:

* $3 \le \text{s.length} \le 5 \times 10^4$
* `s` only consists of `a`, `b` or `c` characters.



## 题目解读

&emsp;统计包含三个字符的子串数目。

```java
class Solution {
    public int numberOfSubstrings(String s) {

    }
}
```

## 程序设计

* 仔细观察，发现包含三个子字符的最短序列加前面的字符（即可组成的组合数）就是当前的子串数目，问题转换为求当前遍历位置的最短子串。

```java
class Solution {
    public int numberOfSubstrings(String s) {
        if (s == null || s.isEmpty()) return 0;

        int count = 0;
        char[] str = s.toCharArray();
        // 队列计数
        int[] counter = new int[3];
        // 队列索引
        int left = 0, right = 0;
        while (right < s.length()) {
            counter[str[right++] - 'a']++;

            // 队列中存在所需字符
            if (counter[0] > 0 && counter[1] > 0 && counter[2] > 0) {
                // 出队求最短序列
                while (counter[str[left] - 'a'] > 1) {
                    counter[str[left++] - 'a']--;
                }
                // 当前位置结尾的所有符合要求子串
                count += left + 1;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了95.21%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。