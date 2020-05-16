[toc]

Write a SQL query to find all duplicate emails in a table named `Person`.

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

For example, your query should return the following for the above table:

```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

Note: All emails are in lowercase.



## 题目解读

&emsp;查询存在重复的数据。

```sql
# Write your MySQL query statement below
```

## 程序设计

* `GROUP BY`和`count()`函数组合。

```sql
SELECT TEMP.Email FROM (SELECT Email, count(Email) AS COUNT FROM Person GROUP BY Email) TEMP WHERE TEMP.COUNT > 1;
```

## 性能分析

执行用时：247ms，在所有MySQL提交中击败了33.32%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了`GROUP BY`和`HAVING`的组合。

```sql
SELECT Email FROM Person GROUP BY Email HAVING count(Email) > 1;
```

执行用时：252ms，在所有MySQL提交中击败了31.85%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。