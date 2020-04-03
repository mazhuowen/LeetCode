[toc]

Given a **non-empty** string s and a dictionary `wordDict` containing a list of **non-empty** words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

Note:

* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.



## 题目解读

&emsp;定一个**非空**字符串和一个包含**非空**单词列表的字典，判定字符串是否可以被空格拆分为一个或多个在字典中出现的单词。

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {

    }
}
```

## 程序设计

* 最基本的思路是从头遍历字符串，如果包含则递归遍历后半部分字符串。

```java
class Solution {
    private Set<String> set;

    public boolean wordBreak(String s, List<String> wordDict) {
        if (s == null || s.isEmpty() || wordDict == null || wordDict.isEmpty()) return false;
        set = new HashSet<>(wordDict);

        return wordBreak(s, 0);
    }
	// 递归判断字符串
    private boolean wordBreak(String s, int start) {
        if (start >= s.length()) return true;
	
        // 遍历后面的字符串，若匹配，则递归遍历剩余的字符串
        for (int end = start + 1; end <= s.length(); end++) {
            if (set.contains(s.substring(start, end)) && wordBreak(s, end)) {
                return true;
            }
        }
        return false;
    }
}
```

* 上面的方法在极端情况，如字符串为`aaaa…b`，而字典为`a`、`aa`、`aaa`、`…`，则在最外层递归，需要遍历数组直到末尾，每遍历一位，由于前缀全在字典，故会递归调用，$T(N) = T(N - 1) + T(N - 2) + \dots + T(1) \implies T(N) = 2T(N - 1)$，时间复杂度为$O(2^N)$，空间复杂度为$O(N)$，会超时。上述方法存在重复判断的问题，可引入额外空间存储中间结果。
* 由于遍历从左往右，然后结果从右往左返回，需要记录返回的结果，这样下次遍历如有结果，进而直接返回。

```java
class Solution {
    private Set<String> set;
    // 记录该位置之后的字符串是否可由字典中元素构成
    int[] record;

    public boolean wordBreak(String s, List<String> wordDict) {
        if (s == null || s.isEmpty() || wordDict == null || wordDict.isEmpty()) return false;
        set = new HashSet<>(wordDict);
        record = new int[s.length()];

        return wordBreak(s, 0);
    }
	// 递归判断字符串
    private boolean wordBreak(String s, int start) {
        if (start >= s.length()) return true;
	
        if (record[start] != 0) return record[start] == 1; 

        // 遍历后面的字符串，若匹配，则递归遍历剩余的字符串
        for (int end = start + 1; end <= s.length(); end++) {
            if (set.contains(s.substring(start, end)) && wordBreak(s, end)) {
                // 更新结果
                record[start] = 1;
                return true;
            }
            // 更新结果
            record[start] = -1;
        }
        return false;
    }
}
```

* 有了上述分析，可以直接使用动态规划解答：

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if (s == null || s.isEmpty() || wordDict == null || wordDict.isEmpty()) return false;

        Set<String> set = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && set.contains(s.substring(j, i))) {
                    dp[i] = true;
                }
            }
        }
        return dp[s.length()];
    }
}
```

## 性能分析

&emsp;优化后的递归，在最坏情况下，第一次遍历完会把所有结果存储，后续遍历只是简单的取数组值判断，故$T(N) = T(N - 1) + N - 1$，时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：7ms，在所有java提交中击败了73.69%的用户。

内存消耗：39.6MB，在所有java提交中击败了5.29%的用户。

&emsp;动态规划的时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：10ms，在所有java提交中击败了47.31%的用户。

内存消耗：39.7MB，在所有java提交中击败了5.29%的用户。

## 官方解题

&emsp;同上，密切关注。