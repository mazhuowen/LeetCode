[toc]

Given a text file `file.txt` that contains list of phone numbers (one per line), write a one liner bash script to print all valid phone numbers.

You may assume that a valid phone number must appear in one of the following two formats: `(xxx) xxx-xxxx` or `xxx-xxx-xxxx`. (x means a digit)

You may also assume each line in the text file must not contain leading or trailing white spaces.



## 题目解读

&emsp;正则表达式校验电话号码。

## 程序设计

* 考察`grep`参数`-P`。注意括号形式后有一个空格，使用`\s`匹配。

```shell
grep -P '^([\d]{3}-|\([\d]{3}\)\s)[\d]{3}-[\d]{4}$' file.txt
```

## 性能分析

执行用时：0ms，在所有Bash提交中击败了100.00%的用户。

内存消耗：3.1MB，在所有Bash提交中击败了92.86%的用户。

## 官方解题

&emsp;暂无，密切关注。