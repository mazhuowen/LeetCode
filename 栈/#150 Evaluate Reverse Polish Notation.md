[toc]

Evaluate the value of an arithmetic expression in Reverse Polish Notation.Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

Note:

* Division between two integers should truncate toward zero.
* The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.



## 题目解读

&emsp;给定后缀表达式，计算表达结果，假设后缀表达式总是有效的。

```java
class Solution {
    public int evalRPN(String[] tokens) {
        
    }
}
```

## 程序设计

* 从左至右依次遍历表达式，将数字压栈，遇到操作符则弹出栈顶的两个，进行计算，将计算结果压栈，最后遍历完，栈只有一个结果，弹出即可。

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        for(int i = 0; i < tokens.length; i++) {
            // 将数字入栈
            if(!"+".equals(tokens[i]) && !"-".equals(tokens[i]) && !"*".equals(tokens[i]) && !"/".equals(tokens[i])) {
                stack.push(Integer.valueOf(tokens[i]));
            } else {
                // 遇到操作符弹出栈顶两个元素运算
                int num2 = stack.pop();
                int num1 = stack.pop();
                switch (tokens[i]) {
                    case "+":
                        stack.push(num1 + num2);
                        break;
                    case "-":
                        stack.push(num1 - num2);
                        break;
                    case "*":
                        stack.push(num1 * num2);
                        break;
                    case "/":
                        stack.push(num1 / num2);
                        break;
                    default :
                        break;
                }
            }
        }
        // 由于后缀表达式一定有效，最后栈中唯一的元素就是结果
        return stack.pop();
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：9ms，在所有java提交中击败了67.12%的用户。

内存消耗：36.5MB，在所有java提交中击败了95.12%的用户。

## 官方解题

&emsp;暂无，密切关注。社区中思路一致，有些选择自行实现的顺序栈，运行较快。