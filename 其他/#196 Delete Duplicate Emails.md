[toc]

Write a SQL query to **delete** all duplicate email entries in a table named `Person`, keeping only unique emails based on its smallest **Id**.

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id is the primary key column for this table.
```

For example, after running your query, the above `Person` table should have the following rows:

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```



Note:

Your output is the whole `Person` table after executing your sql. Use `delete` statement.



## 题目解读

&emsp;删除重复的邮箱。

## 程序设计

* 自连接查询。

```mysql
DELETE p1 FROM Person p1, Person p2 WHERE p1.Id > p2.Id AND p1.Email = p2.Email;
```

## 性能分析

执行用时：1630ms，在所有MySQL提交中击败了12.38%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;社区时间性能较好的方法县查出不重复的结果，然后删除重复值。

```mysql
delete from Person where Id not in (
    # 一定要包一层，否则报错
    select Id from (
        select min(t2.Id) AS Id from Person t2 group by t2.Email
    ) AS t3
);
```