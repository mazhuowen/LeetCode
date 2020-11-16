[toc]

The `Employee` table holds all employees. Every employee has an Id, and there is also a column for the department Id.
```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
```

The `Department` table holds all departments of the company.
```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

Write a SQL query to find employees who earn the top three salaries in each of the department. For the above tables, your SQL query should return the following rows (order of rows does not matter).
```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```
Explanation:

In IT department, Max earns the highest salary, both Randy and Joe earn the second highest salary, and Will earns the third highest salary. There are only two employees in the Sales department, Henry earns the highest salary while Sam earns the second highest salary.



## 题目解读

&emsp;统计不同部门前三高薪的员工。

## 程序设计

* 使用自连接查询比当前薪水高的薪水不超过$3$个的职工。

```mysql
SELECT 
    d.Name AS Department, e1.Name AS Employee, e1.Salary AS Salary 
FROM 
    Employee e1 
        JOIN 
    Department d ON e1.DepartmentId = d.Id 
# 前三高的薪水，大于该薪水的值不超过3个
WHERE 
    3 > (
        SELECT 
            count(DISTINCT e2.Salary) 
        FROM 
            Employee e2 
        WHERE 
            e1.DepartmentId = e2.DepartmentId 
                AND e2.Salary > e1.Salary
        )
;
```

## 性能分析

执行用时 :913 ms, 在所有 MySQL 提交中击败了32.29%的用户

内存消耗 :0B, 在所有 MySQL 提交中击败了100.00%的用户

## 官方解题

&emsp;上述思路参考官方。