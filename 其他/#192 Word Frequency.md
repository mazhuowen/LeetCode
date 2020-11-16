[toc]

Write a bash script to calculate the frequency of each word in a text file `words.txt`.

For simplicity sake, you may assume:

* `words.txt` contains only lowercase characters and space `' '` characters.
* Each word must consist of lowercase characters only.
* Words are separated by one or more whitespace characters.



**Note**:

* Don't worry about handling ties, it is guaranteed that each word's frequency count is unique.
* Could you write it in one-line using `Unix pipes`?



## 题目解读

&emsp;统计句子中的单词并按照倒序打印。

## 程序设计

* `tr`操作通过分隔符分隔替换，需注意`-s`参数删除多余的空格；`awk`操作需注意符号转义，单引号中空格需要使用双引号表示。

```shell
cat words.txt | tr -s ' ' '\n'|sort|uniq -c|sort -r| awk '{print $2" "$1}'
```

## 性能分析

执行用时：0ms，在所有Bash提交中击败了100.00%的用户。

内存消耗：3.3MB，在所有Bash提交中击败了8.33%的用户。

## 官方解题

&emsp;暂无，密切关注。