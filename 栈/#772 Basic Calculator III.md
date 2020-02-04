[toc]

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the plus `+` or minus sign `-`, non-negative integers and empty spaces .

The expression string contains only non-negative integers, `+`, `-`, `*`, `/` operators , open `(` and closing parentheses `)` and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-2147483648, 2147483647]`.



**Note:** **Do not** use the `eval` built-in library function.



## 题目解读

&emsp;实现基本的计算功能。需注意存在负号表示负数，而不仅仅是减号。负号出现为了与其它操作符区分，一般会出现在表达式开头或开括号后面。

```java
class Solution {
    public int calculate(String s) {
        
    }
}
```

## 程序设计

* 基本的中缀转后缀，后缀计算。特别注意的是表达式中有空格，还有负号表示负数。根据规律负数要么在表达式开头，要么在开括号后面，可以将`-x`替换为`0-x`，将特殊一般化。

```java
class Solution {
    public int calculate(String s) {
        // 表达式开头，体寒
        if(s.charAt(0) == '-') {
            s = "0" + s;
        }
        // 开括号后面
        s = s.replaceAll("\\([ ]*-", "(0-");
        String post = transToPost(s);
        int res = caluPost(post);
        return res;
    }

    // 中缀转后缀表达式
    private String transToPost(String express) {
        Stack<Character> stack = new Stack<>();
        String post = "";
        for(int i = 0; i < express.length(); i++) {
            switch(express.charAt(i)) {
                // 开括号入栈
                case '(':
                    stack.push(express.charAt(i));
                    break;
                // 闭括号出栈前面的操作符
                case ')':
                    while(stack.peek() != '(') {
                        post += stack.pop() + " ";
                    }
                    // 弹出开括号
                    stack.pop();
                    break;
                // 前面的高优先级或相等优先级操作符出栈
                case '+':
                case '-':
                    while(!stack.isEmpty() && stack.peek() != '(') {
                        post += stack.pop() + " ";
                    }
                    stack.push(express.charAt(i));
                    break;
                // 前面的*、/出栈
                case '*':
                case '/':
                    while(!stack.isEmpty() && (stack.peek() == '*' || stack.peek() == '/')) {
                        post += stack.pop() + " ";
                    }
                    stack.push(express.charAt(i));
                    break;
                // 空格忽视
                case ' ':
                    break;
                // 数字
                default:
                    int val = 0;
                    while(i < express.length() && Character.isDigit(express.charAt(i))) {
                        val = val * 10 + (express.charAt(i) - '0');
                        i++;
                    }
                    post += val + " ";
                    i--;
                    break;
            }
        }
        while(!stack.isEmpty()) {
            post += stack.pop() + " ";
        }
        return post;
    }

    // 计算后缀表达式
    private int caluPost(String post) {
        Stack<Integer> stack = new Stack<>();
        String[] operas = post.split(" ");
        for(int i = 0; i < operas.length; i++) {
            switch(operas[i]) {
                case "+":
                case "-":
                case "*":
                case "/":
                    int num2 = stack.pop();
                    int num1 = stack.pop();
                    stack.push(calu(num1, num2, operas[i]));
                    break;
                default:
                    stack.push(Integer.valueOf(operas[i]));
                    break;
            }
        }
        return stack.pop();
    }

    private int calu(int num1, int num2, String opera) {
        switch(opera) {
            case "+":
                return num1 + num2;
            case "-":
                return num1 - num2;
            case "*":
                return num1 * num2;
            case "/":
                return num1 / num2;
            default:
                return 0;
        }
    }
}
```

测试用例：`-1 + 1`，测试负号在开头；`1 + (-1)`测试负号在开括号后。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：29ms，在所有java提交中击败了5.26%的用户。

内存消耗：39.4MB，在所有java提交中击败了51.58%的用户。

## 官方解题

&emsp;暂无，密切关注。