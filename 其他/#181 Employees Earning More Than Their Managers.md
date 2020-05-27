[toc]

The `Employee` table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

Given the `Employee` table, write a SQL query that finds out employees who earn more than their managers. For the above table, Joe is the only employee who earns more than his manager.

```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```



## 题目解读

&emsp;查找比主管薪资高的职员。

## 程序设计

* 自连接查询。

```mysql
SELECT a.Name AS Employee FROM Employee a, Employee b WHERE a.ManagerId = b.Id AND a.Salary > b.Salary;
```

## 性能分析

执行用时：256ms，在所有MySQL提交中击败了96.12%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;官方还提供了级联查询的思路。

```mysql
SELECT a.Name AS Employee FROM Employee a INNER JOIN Employee b ON a.ManagerId = b.Id AND a.Salary > b.Salary;
```

执行用时：254ms，在所有MySQL提交中击败了97.07%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。