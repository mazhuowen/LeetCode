[toc]

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: `+` and `-`, you and your friend take turns to flip two **consecutive** `"++"` into `"--"`. The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to compute all possible states of the string after one valid move.



**Example**:

```
Input: s = "++++"
Output: 
[
  "--++",
  "+--+",
  "++--"
]
```



**Note**: If there is no valid move, return an empty list `[]`.



## 题目解读

&emsp;给出翻转游戏的下一次状态。

```java
class Solution {
    public List<String> generatePossibleNextMoves(String s) {

    }
}
```

## 程序设计

* 将`++`替换为`--`。

```java
class Solution {
    public List<String> generatePossibleNextMoves(String s) {
        List<String> res = new LinkedList<>();
        char[] seq = s.toCharArray();
        for (int i = 0; i < seq.length - 1; i++) {
            if (seq[i] != seq[i + 1] || seq[i] != '+') continue;
            seq[i] =seq[i + 1] = seq[i] == '+' ? '-' : '+';
            res.add(new String(seq));
            seq[i] =seq[i + 1] = seq[i] == '+' ? '-' : '+';
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：38.4 MB, 在所有 Java 提交中击败了99.33%的用户。

## 官方解题

&emsp;官方思路类似。
