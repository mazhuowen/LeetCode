[toc]

Given a string `S`, consider all duplicated substrings: (contiguous) substrings of S that occur 2 or more times.  (The occurrences may overlap.)

Return **any** duplicated substring that has the longest possible length.  (If `S` does not have a duplicated substring, the answer is `""`.)



**Note:**

* $2 \le \text{S.length} \le 10^5$
* `S` consists of lowercase English letters.



## 题目解读

&emsp;找出字符串中最长的重复子串。

```java
class Solution {
    public String longestDupSubstring(String S) {

    }
}
```

## 程序设计

* 最简单的思路是暴力匹配，从长到短开始截取匹配，如果集合中存在该子串，说明是最长子串，返回。但该方法会超出内存限制；在每一轮遍历完后清空内存，又会超出时间限制。

```java
class Solution {
    public String longestDupSubstring(String S) {
        if (S == null || S.isEmpty()) return "";

        // 保存子串
        Set<String> set = new HashSet<>();
        int n = S.length();
        // 从长到短遍历
        for (int len = n - 1; len >= 1; len--) {
            for (int i = 0; i <= n - len; i++) {
                String sub = S.substring(i, i + len);
                if (set.contains(sub)) return sub;
                set.add(sub);
            }
            // 遍历完清空本轮集合
            set.clear();
        }
        return "";
    }
}
```

* 在遍历长度时，从长到短依次进行，实际情况很少有重复子串超过字符串一半的长度，依次遍历浪费性能；其次最长重复子串中的子串也是重复的，这样可以使用二分查找来决定当前长度字符串是否存在重复子串。
* 该方法报超出内存限制。

```java
class Solution {
    public String longestDupSubstring(String S) {
        if (S == null || S.isEmpty()) return "";

        // 二分搜索最大长度
        int left = 1, right = S.length() - 1;
        while (left < right) {
            int mid = left + (right - left) / 2 + 1;
            // 由于区间收缩的关系，当left==right时，temp还是上一轮的结果，可能为null
            String temp = longestDupSubstring(S, mid);
            if (temp.isEmpty()) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        // 由于temp不能反映当前长度的子串，还需要再次遍历
        return longestDupSubstring(S, left);
    }

    private String longestDupSubstring(String S, int len) {
        int n = S.length();
        Set<String> set = new HashSet<>();
        for (int i = 0; i <= n - len; i++) {
            String sub = S.substring(i, i + len);
            if (set.contains(sub)) return sub;
            set.add(sub);
        }
        return "";
    }
}
```

* 考虑到使用集合存储子串占用太多资源，可使用`RK`算法编码字符串，需注意数值溢出的问题，需要取模。

```java
class Solution {
    public String longestDupSubstring(String S) {
        if (S == null || S.isEmpty()) return "";

        char[] str = S.toCharArray();
        int left = 1, right = S.length() - 1;
        while (left < right) {
            int mid = left + (right - left) / 2 + 1;
            int idx = longestDupSubstring(str, mid);
            if (idx == -1) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        
        int start = longestDupSubstring(S.toCharArray(), left);
        return start == -1 ? "" : S.substring(start, start + left);
    }

    private int longestDupSubstring(char[] str, int len) {
        Set<Long> set = new HashSet<>();
        int start = 0, end = 0;
        // 字符串的编码值、模、最高位为1的数值
        long key = 0L, mod = (long)Math.pow(2, 32), high = 1L;
        // 定位窗口
        for (; end < len; end++) {
            // 26进制
            key = (key * 26 + (str[end] - 'a')) % mod;
        }
        set.add(key);
        // 计算最高位为1，其余位置为0的数值（注意长度为len-1）
        for (int i = 0; i < len - 1; i++) {
            high = (26 * high) % mod;
        }

        while (end < str.length) {
            // 减去队头
            key = (key - ((str[start++] - 'a') * high) % mod + mod) % mod;
            // 加入队尾
            key = (key * 26 + (str[end++] - 'a')) % mod;
            if (set.contains(key)) return start;
            set.add(key);
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：398ms，在所有java提交中击败了60.64%的用户。

内存消耗：51.6MB，在所有java提交中击败了75.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。