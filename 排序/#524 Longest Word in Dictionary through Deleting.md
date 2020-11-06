[toc]

Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.



**Note**:

* All the strings in the input will only contain lower-case letters.
* The size of the dictionary won't exceed $1000$.
* The length of all the strings in the input won't exceed $1000$.



## 题目解读

&emsp;给定单词字典和一个字符串，查出能通过删除字符得到的最长单词。

```java
class Solution {
    public String findLongestWord(String s, List<String> d) {
        
    }
}
```

## 程序设计

* 判断单词是否是字符串的子串，可以采用归并排序遍历两个数组的思路进行。由于是最长子串，如果长度相等，则按照字典序。这样可以提前给字典中的单词排序，长的在前面，一样长则字典序小的在前面。

```java
class Solution {
    public String findLongestWord(String s, List<String> d) {
        if(d == null || d.isEmpty()) {
            return "";
        }
        // 排序，根据题意，长的排在前面，一样长则根据字典序排列
        Collections.sort(d, (a, b) -> {
            if(a.length() == b.length()) {
                return a.compareTo(b);
            } else {
                return b.length() - a.length();
            }
        });
        // 从头遍历，遇到的第一个就是答案
        for(String word : d) {
            if(isSubString(s, word)) {
                return word;
            }
        }
        return "";
    }
	// 判断是否是子串
    private boolean isSubString(String s, String word) {
        if(word == null || word.isEmpty() || word.length() > s.length()) {
            return false;
        }
        int i = 0, j = 0;
        while(i < s.length() && j < word.length()) {
            // 当前字符相等，则同时遍历下一个，否则继续遍历s
            if(s.charAt(i++) == word.charAt(j)) {
                j++;
            }
        }
        // 最后word遍历完说明包含该单词
        return j == word.length();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM\log_2N)$，其中$M$为平均单词长度；空间复杂度为$O(1)$。

执行用时：45ms，在所有java提交中击败了19.36%的用户。

内存消耗：42.6MB，在所有java提交中击败了5.29%的用户。

## 官方解题

&emsp;官方还提供了不排序直接比较版本。由于是字符串，排序不一定比直接比较快，具体问题具体对待，还要看数据是怎么样的。

```java
class Solution {
    public String findLongestWord(String s, List<String> d) {
        if(d == null || d.isEmpty()) {
            return "";
        }
        // 直接遍历比较
        String res = "";
        for(String word : d) {
            if(isSubString(s, word) && (word.length() > res.length() || (word.length() == res.length() && word.compareTo(res) < 0))) {
                res = word;
            }
        }
        return res;
    }

    private boolean isSubString(String s, String word) {
        if(word == null || word.isEmpty() || word.length() > s.length()) {
            return false;
        }
        int i = 0, j = 0;
        while(i < s.length() && j < word.length()) {
            if(s.charAt(i++) == word.charAt(j)) {
                j++;
            }
        }
        return j == word.length();
    }
}
```

&emsp;时间复杂度为$O(NM)$；由于保存了一个字符串，空间复杂度为$O(M)$。

执行用时：34ms，在所有java提交中击败了38.98%的用户。

内存消耗：42MB，在所有java提交中击败了5.29%的用户。