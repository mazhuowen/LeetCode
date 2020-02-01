[toc]

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the plus `+` or minus sign `-`, **non-negative** integers and empty spaces .



Note:

* You may assume that the given expression is always valid.
* Do not use the eval built-in library function.



## 题目解读

&emsp;将中缀表达式转化为后缀表达式，并计算后缀表达式。题目做了简化，操作符只有加减，且优先级是相同的，其次需要注意处理空格和连续数字。

```java
class Solution {
    public int calculate(String s) {
    
    }
}
```

## 程序设计

* 将中缀表达式转化为后缀表达式，并计算。由于操作符只有加减，简化了流程。

```java
class Solution {
    public int calculate(String s) {
        return calculatePost(transToPost(s));
    }

    // 转化为后缀表达式
    private String transToPost(String express) {
        int index = 0;
        // 拼接后缀表达式，以空格分开
        StringBuffer post = new StringBuffer();
        Stack<String> stack = new Stack<>();
        for(int i = 0; i < express.length(); i++) {
            switch(express.charAt(i)) {
                // 出栈，直到碰到开括号
                case ')':
                    while(!"(".equals(stack.peek())) {
                        post.append(" " + stack.pop());
                    }
                    stack.pop();
                    break;
                // 将栈中其他操作符弹出，直到遇到开括号
                case '+':
                case '-':
                    while(!stack.isEmpty() && !"(".equals(stack.peek())) {
                        post.append(" " + stack.pop());
                    }
                // 操作符、开括号压栈
                case '(':
                    stack.push(express.charAt(i) + "");
                    break;
                // 空格跳过
                case ' ':
                    break;
                // 数字需要判断是否连续
                default:
                    int end = i;
                    post.append(" ");
                    while(end < express.length() && '0' <= express.charAt(end) && express.charAt(end)  <= '9') {
                        post.append(express.charAt(end));
                        end++;
                    }
                    // 由于for循环i++，故减一
                    i = end - 1;
                    break;
            }
        }
        while(!stack.isEmpty()) {
            post.append(" " + stack.pop());
        }
        return post.toString();
    }

    // 计算后缀表达式
    private int calculatePost(String post){
        String[] posts = post.split(" ");
        Stack<Integer> stack = new Stack<>();
        for(int i = 0; i < posts.length; i++) {
            // 遇到操作符，弹出栈顶两个元素，做运算
            if("+".equals(posts[i])) {
                int num2 = stack.pop();
                int num1 = stack.pop();
                stack.push(num1 + num2);
            } 
            else if("-".equals(posts[i])) {
                int num2 = stack.pop();
                int num1 = stack.pop();
                stack.push(num1 - num2);
            }
            // 将数字压栈，此处注意字符串为空的情况
            else if(!posts[i].isEmpty()) {
                stack.push(Integer.valueOf(posts[i]));
            }
        }
        // 返回最终结果
        return stack.pop();
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：104ms，在所有java提交中击败了20.11%的用户。

内存消耗：54.5MB，在所有java提交中击败了14.53%的用户。

## 代码改进

&emsp;由于操作符只有加减，上面流程可以做简化，在一个方法中实现。当每个数字进栈时，判断栈顶元素是否为操作符，是则弹出操作符和另一个操作数计算，并重复上述判断；每个闭括号进栈，则弹出数字（由于做了上述运算，可以保证括号内只有一个运算结果）和开括号，然后判断栈顶元素，同第一步。这样最后栈中剩余的就是操作数。

```java
class Solution {
    public int calculate(String s) {
        int index = 0;
        Stack<String> stack = new Stack<>();
        while(index < s.length()) {
            String num2;
            switch(s.charAt(index)) {
                // 空格跳过
                case ' ':
                    break;
                // 开括号、操作符进栈
                case '(':
                case '+':
                case '-':
                    stack.push(s.charAt(index) + "");
                    break;
                case ')':
                    // 计算括号内的表达式
                    num2 = insertNum(stack, stack.pop());
                    // 弹出开括号
                    stack.pop();
                    // 取消括号后的数字继续与前面的表达式计算
                    num2 = insertNum(stack, num2);
                    // 运算结果入栈
                    stack.push(num2);
                    break;
                default :
                    // 处理连续数字
                    int end = index;
                    num2 = "";
                    while(end < s.length() && s.charAt(end) != '+' &&  s.charAt(end) != '-' && s.charAt(end) != '(' && s.charAt(end) != ')' && s.charAt(end) != ' '){
                        // 连续数字
                        num2 += s.charAt(end);
                        end++;
                    }
                    num2 = insertNum(stack, num2);
                    stack.push(num2);
                    index = end - 1;
                    break;
            }
            index++;
        }
        // 以上流程保证最终栈中只剩一个结果
        return Integer.valueOf(stack.pop());
    }

    private String insertNum(Stack<String> stack, String num2) {
        // 栈顶是运算符，就计算结果
        while(!stack.isEmpty() && ("+".equals(stack.peek()) || "-".equals(stack.peek()))) {
            String opera = stack.pop();
            String num1 = stack.pop();
            num2 = "+".equals(opera) ? (Integer.valueOf(num1) + Integer.valueOf(num2) + "") : (Integer.valueOf(num1) - Integer.valueOf(num2) + "");
        }
        return num2;
    }
}
```

> 需注意两点：当待插入值是数字时，需要检测栈顶是否是操作符，需要迭代计算直到栈顶是开括号或空为止；当当前值是闭括号时，需要弹出数字，然后进行上述插入数字的操作，最后计算出括号内的结果，然后弹出开括号，最后继续数字的插入操作。

&emsp;时间、空间复杂度未变。

执行用时：89ms，在所有java提交中击败了24.80%的用户。

内存消耗：45MB，在所有java提交中击败了28.12%的用户。

## 官方解题

&emsp;由于操作符只有加减，官方在数字符号上做了处理，将减号变成负号，从而转化为全加算术。

```java
class Solution {
    public int calculate(String s) {

        Stack<Integer> stack = new Stack<Integer>();
        int operand = 0;
        int result = 0; // For the on-going result
        int sign = 1;  // 1 means positive, -1 means negative

        for (int i = 0; i < s.length(); i++) {

            char ch = s.charAt(i);
            if (Character.isDigit(ch)) {

                // Forming operand, since it could be more than one digit
                operand = 10 * operand + (int) (ch - '0');

            } else if (ch == '+') {

                // Evaluate the expression to the left,
                // with result, sign, operand
                result += sign * operand;

                // Save the recently encountered '+' sign
                sign = 1;

                // Reset operand
                operand = 0;

            } else if (ch == '-') {

                result += sign * operand;
                sign = -1;
                operand = 0;

            } else if (ch == '(') {

                // Push the result and sign on to the stack, for later
                // We push the result first, then sign
                stack.push(result);
                stack.push(sign);

                // Reset operand and result, as if new evaluation begins for the new sub-expression
                sign = 1;
                result = 0;

            } else if (ch == ')') {

                // Evaluate the expression to the left
                // with result, sign and operand
                result += sign * operand;

                // ')' marks end of expression within a set of parenthesis
                // Its result is multiplied with sign on top of stack
                // as stack.pop() is the sign before the parenthesis
                result *= stack.pop();

                // Then add to the next operand on the top.
                // as stack.pop() is the result calculated before this parenthesis
                // (operand on stack) + (sign on stack * (result from parenthesis))
                result += stack.pop();

                // Reset the operand
                operand = 0;
            }
        }
        return result + (sign * operand);
    }
}
```

