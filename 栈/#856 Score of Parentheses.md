[toc]

Given a balanced parentheses string S, compute the score of the string based on the following rule:

* `()` has score 1
* `AB` has score `A + B`, where `A` and `B` are balanced parentheses strings.
* `(A)` has score `2 * A`, where `A` is a balanced parentheses string.

Note:

* S is a balanced parentheses string, containing only `(` and `)`.
* $2 \le \text{S.length} \le 50$



## 题目解读

&emsp;给定括号匹配的字符串，根据规则计算分数，实质是遇到闭括号计算之前的分数并入栈的操作。

```java
class Solution {
    public int scoreOfParentheses(String S) {
        
    }
}
```

## 程序设计

* 

```java
class Solution {
    public int scoreOfParentheses(String S) {
        if(S == null || S.isEmpty()) {
            return 0;
        }
        Stack<Integer> stack = new Stack<>();
        for(int i = 0; i < S.length(); i++) {
            char sympol = S.charAt(i);
            switch(sympol) {
                // 开括号入栈特殊标识-1
                case '(':
                    stack.push(-1);
                    break;
                // 闭括号
                case ')':
                    int sum;
                    // 出栈计算()形式
                    if(stack.peek() == -1) {
                        sum = 1;
                    } 
                    // 出栈计算(A + B)形式，由于A、B曾都是括号表达式，最后还要乘2
                    else {
                        sum = 0;
                        while(!stack.isEmpty() && stack.peek() != -1) {
                            sum += stack.pop();
                        }
                        // 乘2
                        sum *= 2;
                    }
                    // 弹出开括号，入栈计算值
                    stack.pop();
                    stack.push(sum);
                    break;
                default:
                    break;
            }
        }
        // 最后栈中是并列的计算值，需要加起来
        int sum = 0;
        while(!stack.isEmpty()) {
            sum += stack.pop();
        }
        return sum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了73.23%的用户。

内存消耗：41.5MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;除了上述栈的思路，官方从发现规律的角度出发，对表达式直接贡献值的是`()`，如果知道`()`的深度，则可以得到当前`()`的最终计算值。

```java
class Solution {
    public int scoreOfParentheses(String S) {
        if(S == null || S.isEmpty()) {
            return 0;
        }
        int dep = 0;
        int sum = 0;
        for(int i = 0; i < S.length(); i++) {
            // 开括号深度加一
            if(S.charAt(i) == '(') {
                dep++;
            }
            // 闭括号深度减一
            else {
                dep--;
                // 判断()
                if(S.charAt(i - 1) == '(') {
                    sum += 1 << dep;
                }
            }
        }
        return sum;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.9MB，在所有java提交中击败了5.03%的用户。