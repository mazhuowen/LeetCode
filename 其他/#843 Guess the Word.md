[toc]

We are given a word list of unique words, each word is 6 letters long, and one word in this list is chosen as **secret**.

You may call `master.guess(word)` to guess a word.  The guessed word should have type `string` and must be from the original list with $6$ lowercase letters.

This function returns an `integer` type, representing the number of exact matches (value and position) of your guess to the secret word.  Also, if your guess is not in the given wordlist, it will return $-1$ instead.

For each test case, you have $10$ guesses to guess the word. At the end of any number of calls, if you have made $10$ or less calls to `master.guess` and at least one of these guesses was the **secret**, you pass the testcase.

Besides the example test case below, there will be $5$ additional test cases, each with $100$ words in the word list.  The letters of each word in those testcases were chosen independently at random from `'a'` to `'z'`, such that every word in the given word lists is unique.



**Note**:  Any solutions that attempt to circumvent the judge will result in disqualification.



## 题目解读

&emsp;给定字符串数组，需要调用接口找出需要的字符串，题目要求调用次数不能超过$10$次。

```java
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface Master {
 *     public int guess(String word) {}
 * }
 */
class Solution {
    public void findSecretWord(String[] wordlist, Master master) {
        
    }
}
```

## 程序设计

* 假设随机取一个字符串`s`，调用接口返回`n`，则表示目标字符串`t`与当前字符串`s`有`n`位是匹配的，可在候选中删除与`s`匹配数不是`n`的字符串，重复这个过程。
* 由于字符串是$6$位，假设返回`n`，过滤后最好的情况是只有一个字符串，就是答案；最坏的情况是除了$n$位，其它位置字符与`s`都不相同，有$25 ^ {6 - n}$种候选解，但这其中存在大量字符相等的字符串，其它位置字符各不相同的只有$26 * (6 - n)$类，只需再次进行匹配就可以保留一类，可以大大减少候选，以此类推，直到最后剩余一条。

```java
class Solution {
    public void findSecretWord(String[] wordlist, Master master) {
        int n = wordlist.length;
        // 候选集合
        Set<String> candidates = new HashSet<>();
        for (String str : wordlist) candidates.add(str);
        // 待删除集合
        Set<String> remove = new HashSet<>();

        while (true) {
            int match = -1;
            String pattern = null;
            for (String str : candidates) {
                match = master.guess(str);
                // 找到一个部分匹配的字符串
                if (match != -1) {
                    pattern = str;
                    break;
                }
                // 完全不匹配，加入待删除字符串
                remove.add(str);
            }
            // 找到
            if (match == 6) return;
            // 过滤候选
            removeCandidate(pattern, match, candidates, remove);
        }
    }

    private int sameValPosition(char[] s, char[] t) {
        int count = 0;
        for (int i = 0; i < s.length; i++) {
            if (s[i] == t[i]) count++;
        }
        return count;
    }

    private void removeCandidate(String pattern, int match, Set<String> candidates, Set<String> remove) {
        // 移除当前字符串
        candidates.remove(pattern);
        char[] s = pattern.toCharArray();
        // 移除与当前字符串匹配不足match个的字符串
        for (String str : candidates) {
            // 将与当前字符串匹配数小于match的删除
            if (sameValPosition(s, str.toCharArray()) != match) remove.add(str);
        }
        candidates.removeAll(remove);
        remove.clear();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了96.49%的用户。

内存消耗：37.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路类似。