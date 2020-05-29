[toc]

Given a list of `words`, list of  single `letters` (might be repeating) and `score` of every character.

Return the maximum score of **any** valid set of words formed by using the given letters (`words[i]` cannot be used two or more times).

It is not necessary to use all characters in `letters` and each letter can only be used once. Score of letters `'a'`, `'b'`, `'c'`, ... ,`'z'` is given by `score[0]`, `score[1]`, ... , `score[25]` respectively.



Constraints:

* $1 \le \text{words.length} \le 14$
* $1 \le \text{words[i].length} \le 15$
* $1 \le \text{letters.length} \le 100$
* $\text{letters[i].length} == 1$
* $\text{score.length} == 26$
* $0 \le \text{score[i]} \le 10$
* `words[i]`, `letters[i]` contains only lower case English letters.



## 题目解读

&emsp;给定若干数量字符及其对应分数，求出可构成给定单词列表中单词集合的最大分数。

```java
class Solution {
    public int maxScoreWords(String[] words, char[] letters, int[] score) {

    }
}
```

## 程序设计

* 统计字符数目，从单词列表回溯尝试。

```java
class Solution {
    int maxScore;

    public int maxScoreWords(String[] words, char[] letters, int[] score) {
        if (words == null || words.length == 0 || letters == null || score == null) return 0;

        // 字符计数
        int[] counter = new int[26];
        for (char c : letters) counter[c - 'a']++;

        scoreWords(words, 0, 0, counter, score);
        return maxScore;
    }

    private void scoreWords(String[] words, int idx, int scores, int[] counter,int[] score) {
        boolean complete = true;
        for (int i = idx; i < words.length; i++) {
            // 试探
            int curScore = getScore(words[i].toCharArray(), counter, score);
            if (curScore != -1) {
                complete = false;
                scoreWords(words, i + 1, scores + curScore, counter, score);
            }
            // 回溯
            backTraceScore(words[i].toCharArray(), counter, score);
        }
		
        // 当前方案已尝试完，更新
        if (complete) maxScore = Math.max(maxScore, scores);
    }

    // 减去单词相应计数
    private int getScore(char[] word, int[] counter, int[] score) {
        // 当前单词是否可由剩余字符构成
        boolean flag = false;
        int count = 0;
        for (char c : word) {
            int idx = c - 'a';
            if (counter[idx] <= 0) flag = true;
            else count += score[idx];

            counter[idx]--;
        }
        return flag ? -1 : count;
    }

    // 回溯添加计数
    private void backTraceScore(char[] word, int[] counter, int[] score) {
        for (char c : word) {
            counter[c - 'a']++;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.15%的用户。

内存消耗：37.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。