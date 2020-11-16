[toc]

Write a SQL query to get the nth highest salary from the `Employee` table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```



## 题目解读

&emsp;返回第`N`大的薪水。

## 程序设计

* 考察`LIMIT`后接索引和长度的知识。需要注意设置变量`n = N - 1`，因为索引从0开始。

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  # 需要设置变量，否则无法直接使用N-1
  SET n = N - 1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT ifnull((SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT n, 1), null)
  );
END
```

## 性能分析

执行用时：250ms，在所有MySQL提交中击败了38.12%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。