[toc]

Given a string `S`, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.



Note:

* `S` will be a string with length between 1 and 12.
* `S` will consist only of letters or digits.





## 题目解读

&emsp;给定字符串`S`，通过将字符串`S`中的每个字母转变大小写，可以获得一个新的字符串。返回所有可能得到的字符串集合。

```java
class Solution {
    public List<String> letterCasePermutation(String S) {

    }
}
```

## 程序设计

* 遇到字母回溯尝试大小写。

```java
class Solution {
    public List<String> letterCasePermutation(String S) {
        List<String> res = new LinkedList<>();
        if (S == null || S.isEmpty()) return res;
        letterCasePermutation(S.toCharArray(), 0, res);
        return res;
    }

    private void letterCasePermutation(char[] s, int idx, List<String> res) {
        if (idx >= s.length) {
            res.add(new String(s));
            return;
        }
        // 字符不变（数字或本字母）
        letterCasePermutation(s, idx + 1, res);
        // 变换字母大小写
        if (Character.isLetter(s[idx])) {
            // 试探
            s[idx] = Character.isLowerCase(s[idx]) ? Character.toUpperCase(s[idx]) : Character.toLowerCase(s[idx]);
            // 回溯
            letterCasePermutation(s, idx + 1, res);
            s[idx] = Character.isLowerCase(s[idx]) ? Character.toUpperCase(s[idx]) : Character.toLowerCase(s[idx]);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(2^N)$。

执行用时：2ms，在所有java提交中击败了85.77%的用户。

内存消耗：40.9MB，在所有java提交中击败了8.33%的用户。

## 官方解题

&emsp;官方还提供了其它思路。