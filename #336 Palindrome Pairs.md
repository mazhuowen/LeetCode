[toc]

Given a list of **unique** words, find all pairs of **distinct** indices `(i, j)` in the given list, so that the concatenation of the two words, i.e. `words[i] + words[j]` is a palindrome.



## 题目解读

&emsp;给定字符串数组，找到字符串对，使得组合为回文。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {

    }
}
```

## 程序设计

* 最基本的，暴力匹配，每次遍历一个单词，尝试将所有单词放在其后，判断是否是回文；不尝试放在其前是因为其他字符串会尝试将当前单词放在其后，即当前单词前面，这样不会造成重复。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        List<List<Integer>> res = new LinkedList<>();
        if (words == null || words.length == 0) return res;
        for (int i = 0; i < words.length; i++) {
            String cur = words[i];
            for (int j = 0; j < words.length; j++) {
                if (i == j) continue;
                // 尝试将j放在i后的字符串
                if (isPalindrome(words[i], words[j])) {
                    List<Integer> temp = new LinkedList<>();
                    temp.add(i);
                    temp.add(j);
                    res.add(temp);
                }
            }
        }
        return res;
    }

    private boolean isPalindrome(String prefix, String suffix) {
        int left = 0, right = suffix.length() - 1;
        // 判断两个字符串的公共部分
        while (left < prefix.length() && right >= 0) {
            if (prefix.charAt(left++) != suffix.charAt(right--)) return false;
        }

        // 其中一个字符串遍历完成，判断另一个字符串
        if (left < prefix.length()) {
            right = prefix.length() - 1;
            while (left < right) {
                if (prefix.charAt(left++) != prefix.charAt(right--)) return false;
            }
        }
        else if (right >= 0) {
            left = 0;
            while (left < right) {
                if (suffix.charAt(left++) != suffix.charAt(right--)) return false;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2M)$，空间复杂度为$O(N^2)$。

执行用时：646ms，在所有java提交中击败了8.96%的用户。

内存消耗：41.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;