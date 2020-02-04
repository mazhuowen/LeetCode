[toc]

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

Note:

* You may assume that the given expression is always valid.
* Do not use the eval built-in library function.



## 题目解读

&emsp;没有开闭括号、不存在负数的基本计算问题。

```java
class Solution {
    public int calculate(String s) {
        
    }
}
```

## 程序设计

&emsp;鉴于没有括号，使用栈将乘除先行计算，最后计算加减。

```java
class Solution {
    public int calculate(String s) {
        Stack<String> stack = new Stack<>();
        for(int i = 0; i < s.length(); i++) {
            switch(s.charAt(i)) {
                case '+':
                case '-':
                case '*':
                case '/':
                    stack.push(s.charAt(i) + "");
                    break;
                case ' ':
                    break;
                // 数字
                default:
                    int num2 = 0;
                    while(i < s.length() && Character.isDigit(s.charAt(i))) {
                        num2 = num2 * 10 + (s.charAt(i) - '0');
                        i++;
                    }
                    while(!stack.isEmpty() && ("*".equals(stack.peek()) || "/".equals(stack.peek()))) {
                        String opera = stack.pop();
                        int num1 = Integer.valueOf(stack.pop());
                        num2 = "*".equals(opera) ? (num1 * num2) : (num1 / num2);
                    }
                    stack.push(num2 + "");
                    i--;
                    break;
            }
        }
        int num1 = 0;
        String opera = "+";
        for(int i = 0; i < stack.size(); i++) {
            int num2 = Integer.valueOf(stack.get(i));
            num1 = "+".equals(opera) ? (num1 + num2) : (num1 - num2);
            if(i + 1 < stack.size()) {
                opera = stack.get(i + 1);
            }
            i++;
        }

        return num1;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：47ms，在所有java提交中击败了49.75%的用户。

内存消耗：51.1MB，在所有java提交中击败了26.78%的用户。

## 官方解题

&emsp;暂无，一切关注。

> 社区中运行较快的方法使用了双栈，一个保存操作数，一个保存操作符；当操作符是加减则之前的操作符出栈并计算，入栈当前操作符；当操作符为乘除，之前的乘除操作符出栈并计算，当前操作符入栈。

```java
class Solution {
    public int calculate(String s) {
        Stack<Character> opera = new Stack<>();
        Stack<Integer> nums = new Stack<>();
        for(int i = 0; i < s.length(); i++) {
            switch(s.charAt(i)) {
                // 优先级低之前的操作符出栈
                case '+':
                case '-':
                    while(!opera.isEmpty()) {
                        int num2 = nums.pop();
                        int num1 = nums.pop();
                        nums.push(calcu(opera.pop(), num1, num2));
                    }
                    // 当前操作符入栈
                    opera.push(s.charAt(i));
                    break;
                // 高优先级
                case '*':
                case '/':
                    while(!opera.isEmpty() && (opera.peek() == '*' || opera.peek() == '/')) {
                        int num2 = nums.pop();
                        int num1 = nums.pop();
                        nums.push(calcu(opera.pop(), num1, num2));
                    }
                    // 当前操作符入栈
                    opera.push(s.charAt(i));
                    break;
                case ' ':
                    break;
                // 数字
                default:
                    int val = 0;
                    while(i < s.length() && Character.isDigit(s.charAt(i))) {
                        val = val * 10 + (s.charAt(i) - '0');
                        i++;
                    }
                    nums.push(val);
                    i--;
                    break;
            }
        }
        // 此时要么是x + y，x - y，要么是x +or- y *or/ z
        while(!opera.isEmpty()) {
            int num2 = nums.pop();
            int num1 = nums.pop();
            nums.push(calcu(opera.pop(), num1, num2));
        }
        return nums.pop();
    }

    private int calcu(char opera, int num1, int num2){
         switch(opera) {
                case '+':
                    return num1 + num2;
                case '-':
                    return num1 - num2;
                case '*':
                    return num1 * num2;
                case '/':
                    return num1 / num2;
                default:
                    return 0;
            }
    }
}
```

执行用时：46ms，在所有java提交中击败了50.65%的用户。

内存消耗：40.6MB，在所有java提交中击败了52.08%的用户。