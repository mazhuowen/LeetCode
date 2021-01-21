[toc]

Table: `Followers`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| follower_id | int  |
+-------------+------+
(user_id, follower_id) is the primary key for this table.
This table contains the IDs of a user and a follower in a following relationship where the follower follows the user.
```


Write an SQL query that will, for each user, return the number of followers.

Return the result table ordered by user_id.

The query result format is in the following example:

 

The query result format is in the following example:

```
Followers table:
+---------+-------------+
| user_id | follower_id |
+---------+-------------+
| 0       | 1           |
| 1       | 0           |
| 2       | 0           |
| 2       | 1           |
+---------+-------------+
Result table:
+---------+----------------+
| user_id | followers_count|
+---------+----------------+
| 0       | 1              |
| 1       | 1              |
| 2       | 2              |
+---------+----------------+
The followers of 0 are {1}
The followers of 1 are {0}
The followers of 2 are {0,1}


```



## 题目解读

&emsp;统计每个用户的关注者数目，并按照用户id排序。

## 程序设计

* `GROUP BY`统计即可。

```mysql
SELECT user_id, count(1) as followers_count FROM Followers GROUP BY user_id ORDER BY user_id;
```

## 性能分析

执行用时：340 ms。

内存消耗：0 B。

## 官方解题

&emsp;暂无，密切关注。
