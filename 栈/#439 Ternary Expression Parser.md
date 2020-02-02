[toc]

Given a string representing arbitrarily nested ternary expressions, calculate the result of the expression. You can always assume that the given expression is valid and only consists of digits `0-9`, `?`, `:`, `T` and `F` (`T` and `F` represent True and False respectively).

Note:

* The length of the given string is ≤ 10000.
* Each number will contain only one digit.
* The conditional expressions group right-to-left (as usual in most languages).
* The condition will always be either T or F. That is, the condition will never be a digit.
* The result of the expression will always evaluate to either a digit `0-9`, `T` or `F`.



## 题目解读

&emsp;给定嵌套的条件表达式，得到最后的结果。题目限定条件只能是`T`或`F`，而值是数字或`T`、`F`，总之只有一位。

```java
class Solution {
    public String parseTernary(String expression) {
        
    }
}
```

## 程序设计

* 观察表达式，`?`前必然是条件，`:`前后可以是表达式也可以是单独的值。如果从左至右遍历字符串，遇到的表达式的值可能是后面的表达式；如果从右至左遍历，遇到的第一个字符必然是表达式`:`后面的值，前面必然是另一个值，再前面必然是`?`，可以通过栈算出这个表达式的值，然后以此类推计算出所有的表达式。
* 以`T?T?F:5:3`为例，将`3`、`:`、`5`、`:`、`F`、`?`依次入栈，此时栈顶是`?`，下次遍历元素必然是条件，此时可以将`?`、`F`、`:`、`5`依次出栈，计算出结果`F`，将结果入栈；继续遍历，入栈`?`，当遍历到`T`时栈顶是`?`，将`?`、`F`、`:`、`3`依次出栈，计算出结果`F`，这个结果就是最终结果。

```java
class Solution {
    public String parseTernary(String expression) {
        Stack<Character> stack = new Stack<>();
        char[] express = expression.toCharArray();
        for(int i = express.length - 1; i >= 0; i--) {
            // 当前字符为条件（题目限定为必须是T、F），则出栈
            if(!stack.isEmpty() && stack.peek() == '?') {
                    // 删除？
                    stack.pop();
                    char val1 = stack.pop();
                    // 删去：
                    stack.pop();
                    char val2 = stack.pop();
                    // 加入结果
                    stack.push(express[i] == 'T' ? val1 : val2);
                } 
                // 非条件，直接入栈
                else {
                    stack.push(express[i]);
                }
        }
        return stack.pop() + "";
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：25ms，在所有java提交中击败了42.22%的用户。

内存消耗：38MB，在所有java提交中击败了38.75%的用户。

## 官方解题

&emsp;暂无，密切关注。