[toc]

Given a string containing only three types of characters: `'('`, `')'` and `'*'`, write a function to check whether this string is valid. We define the validity of a string by these rules:

* Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
* Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
* Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
* `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string.
* An empty string is also valid.



**Note:**

The string size will be in the range `[1, 100]`.



## 题目解读

&emsp;检测是否是有效的括号串。

```java
class Solution {
    public boolean checkValidString(String s) {

    }
}
```

## 程序设计

* 最基本的思路是使用一个栈作正常的括号匹配，另一个栈记录星号的索引。当闭括号多出时，判断其前是否有星号可以转化为开括号匹配；当开括号多出时，判断其后是否有星号。

```java
class Solution {
    public boolean checkValidString(String s) {
        if (s == null || s.isEmpty()) return true;
        char[] str = s.toCharArray();

        Stack<Integer> star = new Stack<>();
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < str.length; i++) {
            if (str[i] == '*') star.push(i);
            else if (str[i] == '(') stack.push(i);
            else {
                if (!stack.isEmpty()) stack.pop();
                else {
                    // 闭括号多出，如果之前没有星号，则返回错误
                    if (star.isEmpty()) return false;
                    // 星号作为开括号
                    star.pop();
                }
            }
        }
        // 开括号出多
        while (!stack.isEmpty()) {
            // 如果其后不存在星号，无法匹配
            if (star.isEmpty() || star.pop() < stack.pop()) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了64.30%的用户。

内存消耗：37MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;官方解题巧妙使用贪心算法，使用两个参数记录最小和最大的开括号数。

```java
class Solution {
    public boolean checkValidString(String s) {
        if (s == null || s.isEmpty()) return true;

        int lowLevel = 0, highLevel = 0;
        for (char c : s.toCharArray()) {
            lowLevel += c == '(' ? 1 : -1;
            highLevel += c != ')' ? 1 : -1;

            if (highLevel < 0) return false;
            // 如果开括号少于闭括号，将之前的星号转化为开括号（highlevel大于等于0，说明有足够的星号）
            lowLevel = Math.max(lowLevel, 0);
        }
        return lowLevel == 0;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.4MB，在所有java提交中击败了11.11%的用户。