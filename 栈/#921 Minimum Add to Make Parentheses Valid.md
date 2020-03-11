[toc]

Given a string `S` of `'('` and `')'` parentheses, we add the minimum number of parentheses ( `'('` or `')'`, and in any positions ) so that the resulting parentheses string is valid.

Formally, a parentheses string is valid if and only if:

* It is the empty string, or
* It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
* It can be written as `(A)`, where `A` is a valid string.

Given a parentheses string, return the minimum number of parentheses we must add to make the resulting string valid.

**Note:**

* $\text{S.length} \le 1000$
* `S` only consists of `'('` and `')'` characters.

 

## 题目解读

&emsp;给定字符串，只包含括号，判断需要添加的最少括号数目使得字符串平衡。

```java
class Solution {
    public int minAddToMakeValid(String S) {

    }
}
```

## 程序设计

* 最基本的想法是使用变量计数当前层数，如果大于0表示开括号过剩，继续遍历等待后续闭括号匹配，如果最后大于0，表示需要相等数目的闭括号；如果小于0，则表示前面缺少开括号，需要补充。

```java
class Solution {
    public int minAddToMakeValid(String S) {
        if(S == null || S.isEmpty()) {
            return 0;
        }
        int count = 0;
        // 层数
        int level = 0;
        for(char c : S.toCharArray()) {
            if(c == '(') {
                // 前面字符串缺失开括号
                if(level < 0) {
                    count -= level;
                    level = 0;
                }
                level++;
            } else {
                level--;
            }
        }
        // 最后不平衡，补充括号
        if (level < 0) {
            count -= level;
        } else {
            count += level;
        }
        return count;
    }
}
```

* 可以使用栈实现上述思路，栈中只保存开括号。

```java
class Solution {
    public int minAddToMakeValid(String S) {
        if(S == null || S.isEmpty()) {
            return 0;
        }
        int count = 0;
        Stack<Character> stack = new Stack<>();
        for(char c : S.toCharArray()) {
            // 无匹配的开括号则补充
            if(c == ')') {
                if(!stack.isEmpty()) {
                    stack.pop();
                } else {
                    count++;
                }
            } else {
                stack.push(c);
            }
        }
        // 开括号过剩，补充闭括号
        if(!stack.isEmpty()) {
            count += stack.size();
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB，在所有java提交中击败了5.47%的用户。

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

行用时：1ms，在所有java提交中击败了82.71%的用户。

内存消耗：37.9MB，在所有java提交中击败了5.47%的用户。

## 官方解题

&ensp;同上。