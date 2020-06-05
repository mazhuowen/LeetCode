[toc]

The `Employee` table holds all employees. The employee table has three columns: Employee Id, Company Name, and Salary.

```
+-----+------------+--------+
|Id   | Company    | Salary |
+-----+------------+--------+
|1    | A          | 2341   |
|2    | A          | 341    |
|3    | A          | 15     |
|4    | A          | 15314  |
|5    | A          | 451    |
|6    | A          | 513    |
|7    | B          | 15     |
|8    | B          | 13     |
|9    | B          | 1154   |
|10   | B          | 1345   |
|11   | B          | 1221   |
|12   | B          | 234    |
|13   | C          | 2345   |
|14   | C          | 2645   |
|15   | C          | 2645   |
|16   | C          | 2652   |
|17   | C          | 65     |
+-----+------------+--------+
```

Write a SQL query to find the median salary of each company. Bonus points if you can solve it without using any built-in SQL functions.

```
+-----+------------+--------+
|Id   | Company    | Salary |
+-----+------------+--------+
|5    | A          | 451    |
|6    | A          | 513    |
|12   | B          | 234    |
|9    | B          | 1154   |
|14   | C          | 2645   |
+-----+------------+--------+
```



## 题目解读

&emsp;求每个部门薪资中位数。

## 程序设计

*  参考官方解题，先编号再查询。

```mysql
SELECT
    a.Id,
    a.Company,
    a.Salary
FROM
	-- 给每个部门的人员薪资从低到高编号
    (
    SELECT
        e.Id,
        e.Company,
        e.Salary,
        IF(@PreCom = e.Company, @Rank := @Rank + 1, @Rank := 1) AS `Rank`,
        @PreCom := e.Company
    FROM
        -- 变量初始化
        EmPloyee e, (SELECT @Rank := NULL, @PreCom := NULL) temp
    ORDER BY e.Company, e.Salary, e.Id
    ) a LEFT JOIN (
        SELECT
            Company,
            count(*) AS `Count`
        FROM Employee GROUP BY Company
    ) b ON a.Company = b.Company
WHERE
	-- 中位数编号
    a.Rank = floor((b.Count + 1) / 2)
        OR a.Rank = floor((b.Count + 2) / 2)
;
```

## 性能分析

执行用时：259ms，在所有MySQL提交中击败了68.66%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。