[toc]

The **Employee** table holds the salary information in a year.

Write a SQL to get the cumulative sum of an employee's salary over a period of 3 months but exclude the most recent month.

The result should be displayed by 'Id' ascending, and then by 'Month' descending.

Example
Input

```
| Id   | Month | Salary |
| ---- | ----- | ------ |
| 1    | 1     | 20     |
| 2    | 1     | 20     |
| 1    | 2     | 30     |
| 2    | 2     | 30     |
| 3    | 2     | 40     |
| 1    | 3     | 40     |
| 3    | 3     | 60     |
| 1    | 4     | 60     |
| 3    | 4     | 70     |
```
Output
```
| Id   | Month | Salary |
| ---- | ----- | ------ |
| 1    | 3     | 90     |
| 1    | 2     | 50     |
| 1    | 1     | 20     |
| 2    | 1     | 20     |
| 3    | 3     | 100    |
| 3    | 2     | 40     |
```


Explanation
Employee '1' has 3 salary records for the following 3 months except the most recent month '4': salary 40 for month '3', 30 for month '2' and 20 for month '1'
So the cumulative sum of salary of this employee over 3 months is 90(40+30+20), 50(30+20) and 20 respectively.

```
| Id   | Month | Salary |
| ---- | ----- | ------ |
| 1    | 3     | 90     |
| 1    | 2     | 50     |
| 1    | 1     | 20     |
```
Employee '2' only has one salary record (month '1') except its most recent month '2'.
```
| Id   | Month | Salary |
| ---- | ----- | ------ |
| 2    | 1     | 20     |
```


Employ '3' has two salary records except its most recent pay month '4': month '3' with 60 and month '2' with 40. So the cumulative salary is as following.
```
| Id   | Month | Salary |
| ---- | ----- | ------ |
| 3    | 3     | 100    |
| 3    | 2     | 40     |
```



## 题目解读

&emsp;查询除最近一个月（即每个员工的最大月）之外，剩下每个月的近三个月的累计薪水（不足三个月也要计算）。

## 程序设计

* 首先采用自连接查询每个月的近三个月的累计薪水，然后除去最大月。

```mysql
SELECT t.Id As id, t.Month AS month, t.Salary FROM 
    (
        SELECT e1.Id, e1.Month, sum(e2.Salary) AS Salary FROM Employee e1, Employee e2 
        WHERE e1.Id = e2.Id AND e1.Month >= e2.Month AND e1.Month - e2.Month < 3
        GROUP BY e1.Id, e1.Month
    ) t, 
    (
        SELECT Id, max(Month) AS Month FROM Employee GROUP BY Id
    ) m
WHERE t.Id = m.Id AND t.Month != m.Month
ORDER BY t.Id ASC, t.Month DESC
;
```

## 性能分析

执行用时：260ms，在所有MySQL提交中击败了65.16%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。