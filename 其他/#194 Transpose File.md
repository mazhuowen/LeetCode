[toc]

Given a text file `file.txt`, transpose its content.

You may assume that each row has the same number of columns and each field is separated by the `' '` character.



## 题目解读

&emsp;转置文件行列。

## 程序设计

* 可转置存储，第一行的每一列字符串是新的行的起始，其后行对应的列拼接在第一行列后面。
* `awk`一行一行地处理文本文件，运行流程是：
  * 先运行`BEGIN`后的`{Action}`；
  * 再运行`{Action}`中的文件处理主体命令；
  * 最后运行`END`后的`{Action}`中的命令。
* 本题用到的`awk`常量：`NF`是当前行的字段序号；`NR`是正在处理的行号。

```shell
awk '{
    for (i = 1; i <= NF; i++) {
        # 第一行的列是各自行的起始
        if (NR == 1) res[i] = $i
        # 后续行拼接在相应的列后
        else res[i] = res[i]" "$i
    }
} END {
	# 打印
    for (j = 1; j <= NF; j++) {
        print(res[j])
    }
}' file.txt
```

## 性能分析

执行用时：4ms，在所有Bash提交中击败了99.64%的用户。

内存消耗：3.5MB，在所有Bash提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。