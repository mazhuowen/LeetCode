[toc]

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

<img src="../images/#17.png" style="zoom:120%;" />

Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.



## 题目解读

&emsp;每个数字可能对应手机按键上的任意一个字母，根据给定的数字序列，输出所有可能的字母组合。

```java
class Solution {
    public List<String> letterCombinations(String digits) {

    }
}
```

## 程序设计

* 枚举所有可能的组合，每次枚举结束都需要回溯。

```java
class Solution {
     char[][] map = new char[10][];

     {
        map[2] = new char[]{'a', 'b' ,'c'};
        map[3] = new char[]{'d', 'e', 'f'};
        map[4] = new char[]{'g', 'h', 'i'};
        map[5] = new char[]{'j', 'k', 'l'};
        map[6] = new char[]{'m', 'n', 'o'};
        map[7] = new char[]{'p', 'q', 'r', 's'};
        map[8] = new char[]{'t', 'u', 'v'};
        map[9] = new char[]{'w', 'x', 'y', 'z'};
     }

    public List<String> letterCombinations(String digits) {
        List<String> res = new LinkedList<>();
        if (digits == null || digits.isEmpty()) return res;

        letterCombinations(digits, 0, new StringBuffer(),map, res);
        return res;
    }

    private void letterCombinations(String digits, int start, StringBuffer s, char[][] map, List<String> res) {
        if (start == digits.length()) {
            res.add(s.toString());
            return;
        }
        for (char c : map[digits.charAt(start) - '0']) {
            letterCombinations(digits, start + 1, s.append(c), map, res);
            // 回溯
            s.setLength(s.length() - 1);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(3^N + 4^M)$，空间复杂度递归为为$O(N + M)$，最外层方法需要保存$O(3^N + 4^M)$个结果，故空间复杂度为$O(3^N + 4^M)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.5MB，在所有java提交中击败了5.21%的用户。

## 官方解题

&emsp;同上。