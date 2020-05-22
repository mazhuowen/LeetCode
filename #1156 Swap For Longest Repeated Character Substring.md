[toc]

Given a string `text`, we are allowed to swap two of the characters in the string. Find the length of the longest substring with repeated characters.



Constraints:

* $1 \le \text{text.length} \le 20000$
* `text` consist of lowercase English characters only.



## 题目解读

&emsp;如果字符串中的所有字符都相同，那么这个字符串是单字符重复的字符串。给定字符串`text`，只能交换其中两个字符一次或者什么都不做，然后得到一些单字符重复的子串。返回其中最长的子串的长度。

```java
class Solution {
    public int maxRepOpt1(String text) {

    }
}
```

## 程序设计



```java
class Solution {
    public int maxRepOpt1(String text) {
        if (text == null || text.length() == 0) return 0;
        char[] s = text.toCharArray();

        // 统计每个字母出现的最前和最后的位置
        int[] first = new int[26];
        int[] last = new int[26];
        Arrays.fill(first, Integer.MAX_VALUE);
        Arrays.fill(last, -1);
        for (int i = 0; i < s.length; i++) {
            int idx = s[i] - 'a';
            last[idx] = i;
            if (first[idx] == Integer.MAX_VALUE) first[idx] = i;
        }

        int maxLen = 0;
        // 队列中最多两个字符，且其中一个计数为1
        Character c1 = null, c2 = null;
        int count1 = 0, count2 = 0;
        // 队列首尾
        int left = 0, right = 0;
        while (right < s.length) {
            // 当前字符为队列主要元素（即计数不为1的元素），加入
            if (c1 == null || s[right] == c1 && count2 <= 1) {
                c1 = s[right];
                count1++;
            } else if (c2 == null  || s[right] == c2 && count1 <= 1) {
                c2 = s[right];
                count2++;
            }
            // 当前字符为队列次要元素（即计数为1的元素），判断出队直到有一个计数为1
            else if (s[right] == c1) {
                int len = right - left;
                // 如果队列只有一种字符或前后存在可交换的字符，则当前长度就是队列长度，否则长度减一（将队列尾字符替换计数为1的字符）
                if (last[c2 - 'a'] > right || first[c2 - 'a'] < left) maxLen = Math.max(maxLen, len);
                else maxLen = Math.max(maxLen, len - 1);

                count1++;
                while (left < s.length && count1 > 1 && count2 > 1) {
                    if (s[left++] == c1) count1--;
                    else count2--;
                }
            } else if (s[right] == c2) {
                int len = right - left;
                if (last[c1 - 'a'] > right || first[c1 - 'a'] < left) maxLen = Math.max(maxLen, len);
                else maxLen = Math.max(maxLen, len - 1);

                count2++;
                while (left < s.length && count1 > 1 && count2 > 1) {
                    if (s[left++] == c1) count1--;
                    else count2--;
                }
            }
            // 当前字符不在队列中，则出队直到有一个字符计数为0
            else {
                int len = right - left;
                if (last[c1 - 'a'] > right || first[c1 - 'a'] < left) maxLen = Math.max(maxLen, len);
                else maxLen = Math.max(maxLen, len - 1);

                while (left < s.length && count1 != 0 && count2 != 0) {
                    if (s[left++] == c1) count1--;
                    else count2--;
                }
                // 将新的字符加入计数为0的字符
                if (count1 == 0) {
                    c1 = s[right];
                    count1++;
                } else {
                    c2 = s[right];
                    count2++;
                }
            }

            right++;
        }
        int len = right - left;
        if (count1 == 0 || count2 == 0 || count1 <= 1 && first[c2 - 'a'] < left || count2 <= 1 && first[c1 - 'a'] < left) maxLen = Math.max(maxLen, len);
        else maxLen = Math.max(maxLen, len - 1);
        return maxLen;
    }
}

```

## 性能分析



## 官方解题