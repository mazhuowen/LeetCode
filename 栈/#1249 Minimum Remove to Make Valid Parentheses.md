[toc]

Given a string s of `'('` , `')'` and lowercase English characters. 

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting parentheses string is valid and return **any** valid string.

Formally, a parentheses string is valid if and only if:

* It is the empty string, contains only lowercase characters, or
* It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
* It can be written as `(A)`, where `A` is a valid string.



Constraints:

* $1 \le \text{s.length} \le 10^5$
* `s[i]` is one of  `'('` , `')'` and lowercase English letters.



## 题目解读

&emsp;给定括号字符串，移除多余的括号，使得最后得到的字符串平衡，要求移除最少的括号数目。

```java
class Solution {
    public String minRemoveToMakeValid(String s) {

    }
}
```

## 程序设计

* 最基本的思路是利用栈来保存遍历过的括号，遇到匹配的则出栈，否则进栈，最后栈中是待删除括号。

```java
class Solution {
    public String minRemoveToMakeValid(String s) {
        if (s == null || s.isEmpty()) return s;

        char[] strs = s.toCharArray();
        Stack<Integer> stack = new Stack<>();
        for (int i = strs.length - 1; i >= 0; i--) {
            if (strs[i] == ')') stack.push(i);
            else if (strs[i] == '(') {
                if (!stack.isEmpty() && strs[stack.peek()] == ')') stack.pop();
                else stack.push(i);
            }
        }

        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < strs.length; i++) {
            while (!stack.isEmpty() && i > stack.peek()) stack.pop();

            if (!stack.isEmpty() && i == stack.peek()) {
                stack.pop();
                continue;
            }
            sb.append(strs[i]);
        }
        return sb.toString();
    }
}
```

* 使用数组模拟栈加速。

```java
class Solution {
    public String minRemoveToMakeValid(String s) {
        if (s == null || s.isEmpty()) return s;

        char[] strs = s.toCharArray();
        // 模拟栈
        int size = 0;
        int[] stack = new int[strs.length];
        for (int i = strs.length - 1; i >= 0; i--) {
            if (strs[i] == ')') stack[size++] = i;
            else if (strs[i] == '(') {
                if (size > 0 && strs[stack[size - 1]] == ')') size--;
                else stack[size++] = i;
            }
        }

        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < strs.length; i++) {
            while (size > 0 && i > stack[size - 1]) size--;

            // 当前字符是待删除字符
            if (size > 0 && i == stack[size - 1]) {
                size--;
                continue;
            }
            sb.append(strs[i]);
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：44ms，在所有java提交中击败了20.52%的用户。

内存消耗：41.1MB，在所有java提交中击败了16.67%的用户。

&emsp;加速后：

执行用时：12ms，在所有java提交中击败了97.39%的用户。

内存消耗：40.4MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;除了上述栈的思路，还有两次遍历法，利用顺序扫描括号对，开阔号总是大于等于闭括号，逆序反之的性质，扫描两次，分别删除多余的闭括号和开括号。