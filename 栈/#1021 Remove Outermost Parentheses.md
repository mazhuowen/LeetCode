[toc]

A valid parentheses string is either empty `("")`, `"(" + A + ")"`, or `A + B`, where `A` and `B` are valid parentheses strings, and `+` represents string concatenation.  For example, `""`, `"()"`, `"(())()"`, and `"(()(()))"` are all valid parentheses strings.

A valid parentheses string `S` is **primitive** if it is nonempty, and there does not exist a way to split it into `S = A+B`, with `A` and `B` nonempty valid parentheses strings.

Given a valid parentheses string `S`, consider its primitive decomposition:` S = P_1 + P_2 + ... + P_k`, where `P_i` are primitive valid parentheses strings.

Return `S` after removing the outermost parentheses of every primitive string in the primitive decomposition of `S`.



**Note:**

* $\text{S.length} \le 10000$
* `S[i]` is `"("` or `")"``
* ``S` is a valid parentheses string



## 题目解读

&emsp;如果有效字符串`S`非空，且不存在将其拆分为`S = A+B`的方法，称其为**原语（primitive）**。对一个字符串原语化就是删除分解后每个原语字符串的最外层括号。

```java
class Solution {
    public String removeOuterParentheses(String S) {

    }
}
```

## 程序设计

* 初步想法是引入栈，栈中只保留最外层开括号，遇到相应的闭括号则出栈删除，中间的所有字符直接拼接即可。但这个想法有个问题，需要判断闭括号是子串中的还是最外层对应的，为了方便判断还需要记录括号的层数。
* 若引入括号层数，会发现栈是多余的。遇到开括号层数加一，闭括号减一，当层数为0时不拼接，其它情况拼接。

```java
class Solution {
    public String removeOuterParentheses(String S) {
        if(S == null || S.isEmpty()) {
            return S;
        }
        // 记录层数
        int level = 0;
        StringBuffer res = new StringBuffer();
        for(char c : S.toCharArray()) {
            // 遇到开括号层数加一，闭括号层数减一
            if(c == '(') {
                // 层数为0表示最外层，不添加
                if(level != 0) {
                    res.append(c);
                }
                level++;
            } else {
                level--;
                if(level != 0) {
                    res.append(c);
                }
            }
        }
        return res.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了80.28%的用户。

内存消耗：38.6MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;暂无，密切关注。