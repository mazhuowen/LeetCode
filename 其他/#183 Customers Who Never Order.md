[toc]

Suppose that a website contains two tables, the `Customers` table and the `Orders` table. Write a SQL query to find all customers who never order anything.

Table: `Customers`.

```
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

Table: `Orders`.

```
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

Using the above tables as example, return the following:

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```



## 题目解读

&emsp;查询没有点餐的顾客。

## 程序设计

* 使用`NOT IN`筛选。

```mysql
SELECT Name AS Customers FROM Customers WHERE Id NOT IN (SELECT CustomerId FROM Orders);
```

* 使用`LEFT JOIN`。

```mysql
SELECT a.Name AS Customers FROM (SELECT c.Name, o.CustomerId FROM Customers c LEFT JOIN Orders o ON c.Id = o.CustomerId) a WHERE a.CustomerId is null;
```

## 性能分析

执行用时：346ms，在所有MySQL提交中击败了34.49%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。