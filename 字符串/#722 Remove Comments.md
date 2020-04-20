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



```java

```

## 性能分析



## 官方解题