[toc]

Given a text file `file.txt`, print just the 10th line of the file.



## 题目解读

&emsp;返回第十行。

## 程序设计

* 最基本的思路，使用`awk`打印第十行。其中`$0`表示整行。

```shell
awk '{if (NR == 10) print($0)}' file.txt
```

## 性能分析

执行用时：4ms，在所有Bash提交中击败了79.65%的用户。

内存消耗：3.6MB，在所有Bash提交中击败了87.88%的用户。

## 官方解题

&emsp;暂无，密切关注。