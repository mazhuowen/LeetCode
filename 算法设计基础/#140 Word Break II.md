[toc]

Given a **non-empty** string s and a dictionary wordDict containing a list of **non-empty** words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

Note:

* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.



## 题目解读

&emsp;将一个非空字符串拆分为一个句子，句子中的单词都是字典中的单词，输出满足条件的句子。

```java
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {

    }
}
```

## 程序设计

* 修改[#139 Word Break](./#139 Word Break.md)中的动态数组为链表，得到如下思路，会超时。

```java
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        if (s == null || wordDict == null || s.isEmpty() || wordDict.isEmpty()) return new LinkedList<>();

        int n = s.length();
        Set<String> set = new HashSet<>(wordDict);
        List<String>[] dp = new LinkedList[n + 1];
        // 保存0～i之间可形成的句子列表
        dp[0] = new LinkedList<>();

        for (int i = 1; i <= n; i++) {
            for (int j = i; j >= 0; j--) {
                // 添加拼接
                if (dp[j] != null && set.contains(s.substring(j, i))) {
                    if (dp[i] == null) dp[i] = new LinkedList<>();
                    // 为空说明是结尾
                    if (dp[j].isEmpty()) dp[i].add(s.substring(j, i));
                    else {
                        for (String prePart : dp[j]) {
                            dp[i].add(prePart + " " + s.substring(j, i));
                        }
                    }
                }
            }
        }
        return dp[n] == null ? new LinkedList<>() : dp[n];
    }
}
```

* 改写为存储中间结果的回溯，通过。

```java
class Solution {
    Set<String> set;
    Map<Integer, List<String>> record;

    public List<String> wordBreak(String s, List<String> wordDict) {
        List<String> res = new LinkedList<String>();
        if (s == null || wordDict == null || s.isEmpty() || wordDict.isEmpty()) return res;

        this.set = new HashSet<>(wordDict);
        // 保存idx之后的字符串可形成的句子列表
        this.record = new HashMap<>();

        res = wordBreak(s, 0);
        return res;
    }

    private List<String> wordBreak(String s, int idx) {
        List<String> res = new LinkedList<>();
        if (idx >= s.length()) {
            res.add("");
            return res;
        }

        if (record.get(idx) != null) return record.get(idx);

        for (int i = idx + 1; i <= s.length(); i++) {
            if (set.contains(s.substring(idx, i))) {
                List<String> temp = wordBreak(s, i);
                if (!temp.isEmpty()) {
                    for (String t : temp) {
                        if (t.isEmpty()) res.add(s.substring(idx, i) + t);
                        else res.add(s.substring(idx, i) + " " + t);
                    }
                }
            }
        }
        record.put(idx, res);
        return res;
    }
}
```

> 回溯不超时而动态规划超时，不通过案例是最坏情况，即`aaa……aab`这样的序列，到最后才发现匹配失败，而回溯存储的是反向的结果，开始就是从末端开始的，所以可以很快排除这种情况。可见是测试用例造成的。

## 性能分析

&emsp;回溯时间复杂度为$O(N^2)$，空间复杂度为$O(2^N)$。

执行用时：15ms，在所有java提交中击败了51.29%的用户。

内存消耗：40.2MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;官方思路同上。社区对回溯尝试进行了剪枝，即遍历字符串长度不能超过字表最大单词长度。

```java
class Solution {
    // 记录最长单词长度
    int maxLen;
    Set<String> set;
    Map<Integer, List<String>> record;

    public List<String> wordBreak(String s, List<String> wordDict) {
        List<String> res = new ArrayList<String>();
        if (s == null || wordDict == null || s.isEmpty() || wordDict.isEmpty()) return res;

        this.set = new HashSet<>();
        for (String t : wordDict) {
            this.set.add(t);
            this.maxLen = Math.max(this.maxLen, t.length());
        }
        this.record = new HashMap<>();

        res = wordBreak(s, 0);
        return res;
    }

    private List<String> wordBreak(String s, int idx) {
        if (record.get(idx) != null) return record.get(idx);

        List<String> res = new ArrayList<>();
        if (idx >= s.length()) {
            res.add("");
            record.put(idx, res);
            return res;
        }

        // 遍历不超过最大单词长度（剪枝，加速性能）
        for (int i = idx + 1; i <= s.length() && i <= idx + maxLen; i++) {
            String sub = s.substring(idx, i);
            if (set.contains(sub)) {
                for (String t : wordBreak(s, i)) {
                    // 结尾空字符
                    if (t.isEmpty()) res.add(sub);
                    else res.add(sub + " " + t);
                }
                
            }
        }
        record.put(idx, res);
        return res;
    }
}
```

执行用时：7ms，在所有java提交中击败了89.03%的用户。

内存消耗：39.7MB，在所有java提交中击败了7.14%的用户。