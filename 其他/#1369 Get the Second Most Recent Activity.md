[toc]

Table: `UserActivity`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| username      | varchar |
| activity      | varchar |
| startDate     | Date    |
| endDate       | Date    |
+---------------+---------+
This table does not contain primary key.
This table contain information about the activity performed of each user in a period of time.
A person with username performed a activity from startDate to endDate.
```



Write an SQL query to show the **second most recent activity** of each user.

If the user only has one activity, return that one. 

A user can't perform more than one activity at the same time. Return the result table in **any** order.

The query result format is in the following example:

```
UserActivity table:
+------------+--------------+-------------+-------------+
| username   | activity     | startDate   | endDate     |
+------------+--------------+-------------+-------------+
| Alice      | Travel       | 2020-02-12  | 2020-02-20  |
| Alice      | Dancing      | 2020-02-21  | 2020-02-23  |
| Alice      | Travel       | 2020-02-24  | 2020-02-28  |
| Bob        | Travel       | 2020-02-11  | 2020-02-18  |
+------------+--------------+-------------+-------------+

Result table:
+------------+--------------+-------------+-------------+
| username   | activity     | startDate   | endDate     |
+------------+--------------+-------------+-------------+
| Alice      | Dancing      | 2020-02-21  | 2020-02-23  |
| Bob        | Travel       | 2020-02-11  | 2020-02-18  |
+------------+--------------+-------------+-------------+

The most recent activity of Alice is Travel from 2020-02-24 to 2020-02-28, before that she was dancing from 2020-02-21 to 2020-02-23.
Bob only has one record, we just take that one.
```



## 题目解读

&emsp;查询最近的第二次活动，没有则返回最近的第一次活动。

## 程序设计

* 使用`HAVING`条件筛选。

```mysql
SELECT b.* FROM UserActivity a, UserActivity b
WHERE a.username = b.username GROUP BY b.username, b.endDate
# 第二次活动或只有一次活动
HAVING sum(a.endDate > b.endDate) = 1 or count(1) = 1;
```

## 性能分析

执行用时：237ms, 在所有MySQL提交中击败了46.07%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。