[toc]

Table: `Person`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
```

PersonId is the primary key column for this table.
Table: `Address`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
```


AddressId is the primary key column for this table.


Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

```
FirstName, LastName, City, State
```



## 题目解读

&emsp;联表查询。

```sql
# Write your MySQL query statement below
```

## 程序设计

* 联表查询。

```sql
# Write your MySQL query statement below
SELECT p.FirstName AS FirstName, p.LastName AS LastName, a.City AS City, a.State AS State FROM Person p LEFT JOIN Address a ON p.PersonId = a.PersonId;
```

## 性能分析

执行用时：276ms，在所有MySQL提交中击败了22.26%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。