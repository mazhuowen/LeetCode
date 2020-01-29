[toc]

Given a string containing just the characters `(`, `)`, `{`, `}`, `[` and `]`, determine if the input string is valid.

An input string is valid if:

* Open brackets must be closed by the same type of brackets.
* Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.



## 题目解读

&emsp;括号匹配，题目假设输入是有效的，即要么空字符串，要么只包含括号的字符串。

```java
class Solution {
    public boolean isValid(String s) {
        
    }
}
```

## 程序设计

* 字符串从左到右遍历，遇到左开括号则入栈，遇到右闭括号则判断栈顶是否是相应开括号，如果是则出栈，否则括号不匹配。遍历结束栈应该是空的，否则不匹配。

```java
class Solution {
    public boolean isValid(String s) {
        // 转化为字符数组
        char[] array = s.toCharArray();
        Stack<Character> stack = new Stack<>();
        for(int i = 0; i < array.length; i++) {
            switch (array[i]){
                // 开括号入栈
                case '(':
                case '{':
                case '[':
                    stack.push(array[i]);
                    break;
                // 闭括号检查栈顶元素是否是匹配的开括号
                case ')':
                    // 需注意栈空的特殊情况
                    if(stack.isEmpty() || stack.pop() != '(') {
                        return false;
                    }
                    break;
                case '}':
                    if(stack.isEmpty() || stack.pop() != '{') {
                        return false;
                    }
                    break;
                case ']':
                    if(stack.isEmpty() || stack.pop() != '[') {
                        return false;
                    }
                    break;
                default:
                    break;
            }
        }
        // 栈不为空，表示有没有匹配完的开括号
        return stack.isEmpty();
    }
}
```

测试样例：空字符串、只包含闭括号的字符串测试栈为空的特殊情况、普通字符串。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：2ms，在所有java提交中击败了93.89%的用户。

内存消耗：34.5 MB，在所有java 提交中击败了17.85%的用户。

## 官方解题

&emsp;官方思路与上述一直，代码封装了开闭括号的对应关系，使得代码显得简介。