[toc]

You are given a string `s` that consists of lower case English letters and brackets. 

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should **not** contain any brackets.



Constraints:

* $0 \le \text{s.length} \le 2000$
* `s` only contains lower case English characters and parentheses.
* It's guaranteed that all parentheses are balanced.



## 题目解读

&emsp;给定字符串只包含括号和小写字母，括号中的字符串需要翻转。

```java
class Solution {
    public String reverseParentheses(String s) {

    }
}
```

## 程序设计

* 最基本的做法遇到闭括号将栈中字符串出栈翻转并入栈。

```java
class Solution {
    public String reverseParentheses(String s) {
        if(s == null || s.isEmpty()) {
            return s;
        }
        Stack<String> stack = new Stack<>();
        String temp = "";
        for(char c : s.toCharArray()) {
            switch(c) {
                // 入栈之前拼接的字符串和开括号
                case '(':
                    stack.push(temp);
                    // 重置字符串
                    temp = "";
                    stack.push("(");
                    break;
                // 出栈
                case ')':
                    // 弹出栈，直到遇到开括号
                    while(!stack.isEmpty() && !stack.peek().equals("(")) {
                        temp = stack.pop() + temp;
                    }
                    // 弹出开括号
                    stack.pop();
                    stack.push(reverse(temp));
                    // 重置字符串
                    temp = "";
                    break;
                // 字符拼接
                default:
                    temp += c;
                    break;
            }
        }
        // 拼接
        while(!stack.isEmpty()) {
            temp =  stack.pop() + temp;
        }
        return temp;
    }
	// 反转字符串
    private String reverse(String s) {
        if(s.isEmpty()) {
            return s;
        }
        StringBuffer sb = new StringBuffer(s);
        return sb.reverse().toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了12.20%的用户。

内存消耗：38.2MB，在所有java提交中击败了5.06%的用户。

## 官方解题

&emsp;暂无，密切关注。社区性能最好的方法思路同上，但是技巧的不适用栈保存待拼接字符串，而是保存开闭括号的字符数组索引，每次遇到闭括号则在字符数组中交换顺序。最后拼接为字符串输出。

```java
class Solution {
    public String reverseParentheses(String s) {
        if(null == s || 0 == s.length()) {
            return null;
        }
        
        char[] chars = s.toCharArray();
        Stack<Integer> stack = new Stack<>();
        StringBuilder sb = new StringBuilder();
        // 入栈开闭括号的索引
        for(int i = 0; i < chars.length; i++) {
            if(chars[i] == '(') {
                stack.push(i);
            } else if(chars[i] == ')') {
                // 翻转括号内的字符串
                reverse(chars, stack.pop() + 1, i - 1); 
            }
        }
        // 拼接字符
        for(int i = 0; i < chars.length; i++) {
            if(chars[i] != '(' && chars[i] != ')') {
                sb.append(chars[i]);
            }
        }
        
        return sb.toString();
    }
    
    public void reverse(char[] ch, int left, int right) {
        while(left < right) {
            char tmp = ch[left];
            ch[left] = ch[right];
            ch[right] = tmp;
            left++;
            right--;
        }
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.4MB，在所有java提交中击败了5.06%的用户。