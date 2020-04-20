[toc]

Given a C++ program, remove comments from it. The program `source` is an array where `source[i]` is the `i`-th line of the source code. This represents the result of splitting the original source code string by the newline character `\n`.

In C++, there are two types of comments, line comments, and block comments.

The string `//` denotes a line comment, which represents that it and rest of the characters to the right of it in the same line should be ignored.

The string `/*` denotes a block comment, which represents that all characters until the next (non-overlapping) occurrence of `*/` should be ignored. (Here, occurrences happen in reading order: line by line from left to right.) To be clear, the string `/*/` does not yet end the block comment, as the ending would be overlapping the beginning.

The first effective comment takes precedence over others: if the string `//` occurs in a block comment, it is ignored. Similarly, if the string `/*` occurs in a line or block comment, it is also ignored.

If a certain line of code is empty after removing comments, you must not output that line: each string in the answer list will be non-empty.

There will be no control characters, single quote, or double quote characters. For example, source = `"string s = "/* Not a comment. */";"` will not be a test case. (Also, nothing else such as defines or macros will interfere with the comments.)

It is guaranteed that every open block comment will eventually be closed, so `/*` outside of a line or block comment always starts a new comment.

Finally, implicit newline characters can be deleted by block comments. Please see the examples below for details.

After removing the comments from the source code, return the source code in the same format.



Note:

* The length of `source` is in the range `[1, 100]`.
* The length of `source[i]` is in the range `[0, 80]`.
* Every open block comment is eventually closed.
* There are no single-quote, double-quote, or control characters in the source code.



## 题目解读

&emsp;删除代码中的注释。注释分行注释和块注释，块注释可以在代码间出现，行注释可在代码后出现。题目限定不会出现字符串等。

```java
class Solution {
    public List<String> removeComments(String[] source) {

    }
}
```

## 程序设计

* 细节非常多，如行注释前可以有代码，而块注释前后都可以有代码；其次对于块注释，可以跨行，合并时需要把块注释前后的代码合并在一起。
* 如果只考虑根据`/`来判断是行注释还是块注释，会陷入死胡同，因为根据`/`来判断块注释闭合区间`*/`，需要向前判断，除了这个，对于`/*/`、`/*.../*/`的情况根本不能为力；转变思路，块注释结束区间可根据`*`来判断，只需向后判断是否是`/`，而无需向前判断。

```java
class Solution {
    public List<String> removeComments(String[] source) {
        List<String> res = new LinkedList<>();

        // 是否在块注释内
        boolean block = false;
        StringBuffer sb = new StringBuffer();
        // 遍历每行
        for (String line : source) {
            char[] token = line.toCharArray();

            // 遍历寻找/
            int idx = 0;
            while (idx < token.length) {
                // 行注释，后续不用遍历
                if (!block && idx + 1 < token.length && token[idx] == '/' && token[idx + 1] == '/') {
                    break;
                }
                // 块注释起始
                else if (!block && idx + 1 < token.length && token[idx] == '/' && token[idx + 1] == '*') {
                    block = true;
                    idx += 2;
                }
                // 块注释结束
                else if (block && idx + 1 < token.length && token[idx] == '*' && token[idx + 1] == '/') {
                    block = false;
                    idx += 2;
                }
                // 代码或/作为操作符
                else if (!block) {
                    sb.append(token[idx]);
                    idx++;
                }
                // 块代码内部，忽略
                else {
                    idx++;
                }
            }

            if (!block && sb.length() > 0) {
                res.add(sb.toString());
                sb = new StringBuffer();
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述参考官方解题。