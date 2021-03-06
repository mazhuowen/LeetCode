[toc]

The `Trips` table holds all taxi trips. Each trip has a unique Id, while Client_Id and Driver_Id are both foreign keys to the Users_Id at the `Users` table. Status is an ENUM type of (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’).

```
+----+-----------+-----------+---------+--------------------+----------+
| Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at|
+----+-----------+-----------+---------+--------------------+----------+
| 1  |     1     |    10     |    1    |     completed      |2013-10-01|
| 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01|
| 3  |     3     |    12     |    6    |     completed      |2013-10-01|
| 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01|
| 5  |     1     |    10     |    1    |     completed      |2013-10-02|
| 6  |     2     |    11     |    6    |     completed      |2013-10-02|
| 7  |     3     |    12     |    6    |     completed      |2013-10-02|
| 8  |     2     |    12     |    12   |     completed      |2013-10-03|
| 9  |     3     |    10     |    12   |     completed      |2013-10-03| 
| 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03|
+----+-----------+-----------+---------+--------------------+----------+
```


The `Users` table holds all users. Each user has an unique Users_Id, and Role is an ENUM type of (‘client’, ‘driver’, ‘partner’).

```
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
```

Write a SQL query to find the cancellation rate of requests made by unbanned users (both client and driver must be unbanned) between **Oct 1, 2013** and **Oct 3, 2013**. The cancellation rate is computed by dividing the number of canceled (by client or driver) requests made by unbanned users by the total number of requests made by unbanned users.

For the above tables, your SQL query should return the following rows with the cancellation rate being rounded to two decimal places.

```
+------------+-------------------+
|     Day    | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |       0.33        |
| 2013-10-02 |       0.00        |
| 2013-10-03 |       0.50        |
+------------+-------------------+
```



## 题目解读

&emsp;统计乘客或司机的订单取消率。

## 程序设计

* 需注意最后计算的小数四舍五入保留两位小数后数字。

```mysql
SELECT
    v.Day AS `Day`,
    round(sum(CASE v.Status WHEN 'completed' THEN 0 ELSE 1 END) / count(*), 2) AS `Cancellation Rate`
FROM
    (
    SELECT
        t.Status,
        date_format(t.Request_at, '%Y-%m-%d') AS Day
    FROM
        Trips t, Users u1, Users u2
    WHERE
        t.Request_at BETWEEN '2013-10-01' AND '2013-10-03'
            AND t.Driver_Id = u1.Users_Id
            AND u1.Banned = 'No'
            AND t.Client_Id = u2.Users_Id
            AND u2.Banned = 'No'
     ) v
GROUP BY v.Day ORDER BY v.Day
;
```

## 性能分析

执行用时：308ms，在所有MySQL提交中击败了25.46%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。