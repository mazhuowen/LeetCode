[toc]

Given two strings `S` and `T`, return if they are equal when both are typed into empty text editors. `#` means a backspace character.

Note:

* $1 \le \text{S.length} \le 200$
* $1 \le \text{T.length} \le 200$
* `S` and `T` only contain lowercase letters and `#` characters.

Follow up:

* Can you solve it in O(N) time and O(1) space?



## 题目解读

&emsp;给定两串字符串，其中`#`代表删除操作，比较最终两个字符串是否一致。

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        
    }
}
```

## 程序设计

* 最简单的，想到利用栈，遇到`#`弹出栈顶元素。

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        return stringStack(S).equals(stringStack(T));
    }

    private String stringStack(String t) {
        Stack<Character> stack = new Stack<>();
        for(char c : t.toCharArray()) {
            switch(c) {
                // 遇到#号出栈
                case '#':
                    if(!stack.isEmpty()) {
                        stack.pop();
                    }
                    break;
                default:
                    stack.push(c);
                    break;
            }
        }
        StringBuffer sb = new StringBuffer();
        while(!stack.isEmpty()) {
            sb.insert(0, stack.pop());
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：3ms，在所有java提交中击败了64.50%的用户。

内存消耗：34.7MB，在所有java提交中击败了30.94%的用户。

## 官方解题

&emsp;题目进阶要求$O(1)$的空间复杂度，

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                // 对连续的或单独的#计数
                if (S.charAt(i) == '#') {
                    skipS++; 
                    i--;
                }
                // 跳过#号删除的字符
                else if (skipS > 0) {
                    skipS--; 
                    i--;
                }
                // 字符，跳出循环比较
                else break;
            }
            // 字符串T同理
            while (j >= 0) {
                if (T.charAt(j) == '#') {=
                    skipT++;
                    j--;
                }
                else if (skipT > 0) {
                    skipT--; 
                    j--;
                }
                else break;
            }
            // 比较是否相等
            if (i >= 0 && j >= 0 && S.charAt(i) != T.charAt(j))
                return false;
            // 两个字符串其中一个已经遍历完，另一个未遍历完，不相等
            if ((i >= 0) != (j >= 0))
                return false;
            // 继续遍历比较
            i--; 
            j--;
        }
        return true;
    }
}
```

执行用时：1ms，在所有java提交中击败了98.26%的用户。

内存消耗：34.6MB，在所有java提交中击败了38.33%的用户。