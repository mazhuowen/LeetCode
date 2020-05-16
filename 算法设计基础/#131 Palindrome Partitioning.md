[toc]

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.



## 题目解读

&emsp;给出字符串所有的子串是回文的组合。

```java
class Solution {
    public List<List<String>> partition(String s) {

    }
}
```

## 程序设计

* 使用回溯，每次判断当前字符串是否是回文，是回文则可以选择从新开始遍历下一个字符串；不管是不是回文都可以选择继续遍历下一个字符。

```java
class Solution {
    List<List<String>> res;

    public List<List<String>> partition(String s) {
        res = new LinkedList<>();
        if (s == null || s.isEmpty()) return res;

        partition(s.toCharArray(), 0, 0, new ArrayList<>(), new StringBuffer(), true);
        return res;
    }

    private void partition(char[] str, int start, int cur, List<String> temp, StringBuffer sb, boolean isPalindrome) {
        // 回溯尝试结束
        if (cur >= str.length) {
            // 如果最后一个字符串也是回文，则加入答案
            if (isPalindrome) {
                if (sb.length() > 0) temp.add(sb.toString());
                res.add(new LinkedList<>(temp));
            }
            return;
        }
        sb.append(str[cur]);
        // 1、之前字符串是回文串，选择新开始字符串
        if (isPalindrome(str, start, cur)) {
            temp.add(sb.toString());
            partition(str, cur + 1, cur + 1, temp, new StringBuffer(), true);
            temp.remove(temp.size() - 1);
        }
        // 2、选择继续试探当前字符串（不管是不是回文）
        partition(str, start, cur + 1, temp, sb, false);

    }

    private boolean isPalindrome(char[] str, int i, int j) {
        while (i < j) {
            if (str[i++] != str[j--]) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了64.33%的用户。

内存消耗：41MB，在所有java提交中击败了23.53%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好的方法采用预处理提前维护好回文子字符串，避免在回溯时遍历判断。

```java
class Solution {
    List<List<String>> res;
    boolean[][] record;

    public List<List<String>> partition(String s) {
        res = new LinkedList<>();
        if (s == null || s.isEmpty()) return res;

        record = new boolean[s.length()][s.length()];
        init(s, record);

        partition(s, 0, new ArrayList<>());
        return res;
    }

    private void partition(String str, int cur, List<String> temp) {
        if (cur >= str.length()) {
            res.add(new LinkedList<>(temp));
            return;
        }
        for (int i = cur; i < str.length(); i++) {
            if (!record[cur][i]) continue;
            // 满足回文字符串，试探
            temp.add(str.substring(cur, i + 1));
            partition(str, i + 1, temp);
            // 回溯
            temp.remove(temp.size() - 1);
        }
    }

    // 曼彻斯特算法计算回文子串
    private void init(String s, boolean[][] record) {
        int idx = 0;
        char[] T = new char[2 * s.length() + 3];
        T[idx++] = '^';
        T[idx++] = '#';
        for (char c : s.toCharArray()) {
            T[idx++] = c;
            T[idx++] = '#';
        }
        T[idx++] = '$';

        int center = 0, right = 0;
        int[] P = new int[T.length];
        for (int i = 1; i < T.length - 1; i++) {
            if (i < right) P[i] = Math.min(P[2 * center - i], right - i);
            while (T[i - P[i] - 1] == T[i + P[i] + 1]) P[i]++;

            // 更新记录
            for (int len = P[i]; len > 0; len -= 2) {
                record[(i - len) / 2][(i + len - 2) / 2] = true;
            }
            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
        }
    }
}
```

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(N^2)$。

执行用时：2ms，在所有java提交中击败了99.32%的用户。

内存消耗：40.9MB，在所有java提交中击败了23.53%的用户。