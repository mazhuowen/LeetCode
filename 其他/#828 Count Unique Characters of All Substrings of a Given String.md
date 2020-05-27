[toc]

Let's define a function `countUniqueChars(s)` that returns the number of unique characters on `s`, for example if `s = "LEETCODE"` then `"L"`, `"T"`,`"C"`,`"O"`,`"D"` are the unique characters since they appear only once in `s`, therefore `countUniqueChars(s) = 5`.

On this problem given a string `s` we need to return the sum of `countUniqueChars(t)` where `t` is a substring of `s`. Notice that some substrings can be repeated so on this case you have to count the repeated ones too.

Since the answer can be very large, return the answer modulo $10^9 + 7$.



**Constraints:**

- $0 \le \text{s.length} \le 10^4$
- `s` contain upper-case English letters only.



## 题目解读

&emsp;统计给定字符串所有子串

```java
class Solution {
    public int uniqueLetterString(String s) {

    }
}
```

## 程序设计

* 最基本的思路是遍历不同长度的窗口，每个窗口从左往右滑动统计，采用字典计数，集合来统计唯一字符数目。时间复杂度为$O(N^2)$，会超时。

```java
class Solution {
    private final static int MOD = 1_000_000_007;

    public int uniqueLetterString(String s) {
        if (s == null || s.isEmpty()) return 0;

        char[] str = s.toCharArray();
        int res = 0;
        // 子字符串长度
        for (int len = 1; len <= str.length; len++) {
            // 记录队列中数目为１的字符
            Set<Character> set = new HashSet<>();
            // 记录队列中字符计数
            Map<Character, Integer> counter = new HashMap<>();

            int start = 0, end = 0;
            // 初始化队列
            while (end < len - 1) {
                // 计数
                counter.put(str[end], counter.getOrDefault(str[end], 0) + 1);
                // 维护单一字符
                if (counter.get(str[end]) == 1) set.add(str[end]);
                else set.remove(str[end]);

                end++;
            }
            for (; end < str.length; start++, end++) {
                // 入队计数
                counter.put(str[end], counter.getOrDefault(str[end], 0) + 1);
                if (counter.get(str[end]) == 1) set.add(str[end]);
                else set.remove(str[end]);

                res += set.size();
                res %= MOD;

                // 出队
                if (counter.get(str[start]) == 1) counter.remove(str[start]);
                else counter.put(str[start], counter.getOrDefault(str[start], 0) - 1);
                if (counter.getOrDefault(str[start], 0) == 1) set.add(str[start]);
                else set.remove(str[start]);
            }
        }
        return res;
    }
}
```

* 转变思路，不以子串为目标，而是以单个字符为目标，则当前字符位置`cur`在上一个位置`pre`和下一个位置`next`之间是唯一的，超出这个区间则存在重复，计数为0，故当前位置字符唯一的子串只需计算这个区间的组合数。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int uniqueLetterString(String S) {
        if (S == null || S.isEmpty()) return 0;

        char[] str = S.toCharArray();
        // 统计字符在字符串中的所有索引
        Map<Character, List<Integer>> indexes = new HashMap<>();
        for (int i = 0; i < str.length; i++) {
            if (indexes.get(str[i]) == null) indexes.put(str[i], new ArrayList<>());
            indexes.get(str[i]).add(i);
        }

        int count = 0;
        for (List<Integer> list : indexes.values()) {
            // 选择当前字符为唯一字符的计数
            for (int i = 0; i < list.size(); i++) {
                int cur = list.get(i);
                // 当前字符的上一个位置和下一个位置
                int pre = i == 0 ? -1 : list.get(i - 1);
                int next = i == list.size() - 1 ? str.length : list.get(i + 1);

                // 在区间pre~next只有唯一的当前字符，计算组合数（重复计数为0，故cur位置的字符不考虑超出pre、next范围的值）
                count += (cur - pre) * (next - cur);
                count %= MOD;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：22ms，在所有java提交中击败了61.76%的用户。

内存消耗：40.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述参考官方思路。