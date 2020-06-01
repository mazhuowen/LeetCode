[toc]

Table `Accounts`:

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
the id is the primary key for this table.
This table contains the account id and the user name of each account.
```

Table `Logins`:

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| login_date    | date    |
+---------------+---------+
There is no primary key for this table, it may contain duplicates.
This table contains the account id of the user who logged in and the login date. A user may log in multiple times in the day.
```



Write an SQL query to find the id and the name of active users.

Active users are those who logged in to their accounts for 5 or more consecutive days.

Return the result table **ordered** by the id.

The query result format is in the following example:

```
Accounts table:
+----+----------+
| id | name     |
+----+----------+
| 1  | Winston  |
| 7  | Jonathan |
+----+----------+

Logins table:
+----+------------+
| id | login_date |
+----+------------+
| 7  | 2020-05-30 |
| 1  | 2020-05-30 |
| 7  | 2020-05-31 |
| 7  | 2020-06-01 |
| 7  | 2020-06-02 |
| 7  | 2020-06-02 |
| 7  | 2020-06-03 |
| 1  | 2020-06-07 |
| 7  | 2020-06-10 |
+----+------------+

Result table:
+----+----------+
| id | name     |
+----+----------+
| 7  | Jonathan |
+----+----------+
User Winston with id = 1 logged in 2 times only in 2 different days, so, Winston is not an active user.
User Jonathan with id = 7 logged in 7 times in 6 different days, five of them were consecutive days, so, Jonathan is an active user.
```



Follow up question:
Can you write a general solution if the active users are those who logged in to their accounts for n or more consecutive days?



## 题目解读

&emsp;查询连续五天以上登陆的用户名。

## 程序设计

* 参考社区思路，子连接查询时间差值在4以内的时间，然后统计时间是否是5，是则表示存在连续的5个时间。

```mysql
SELECT * FROM Accounts WHERE id in (
    SELECT DISTINCT l1.id FROM Logins l1 JOIN
    Logins l2 ON  l1.id = l2.id
    AND datediff(l1.login_date, l2.login_date) BETWEEN 0 AND 4
    GROUP BY l1.id, l1.login_date
    HAVING count(DISTINCT l2.login_date) = 5
) ORDER BY id;
```

## 性能分析

执行用时：966ms，在所有MySQL提交中击败了84.96%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。