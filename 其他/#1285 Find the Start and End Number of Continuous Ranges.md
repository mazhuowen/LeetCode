[toc]

Table: `Logs`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| log_id        | int     |
+---------------+---------+
id is the primary key for this table.
Each row of this table contains the ID in a log Table.
```

Since some IDs have been removed from Logs. Write an SQL query to find the start and end number of continuous ranges in table Logs.

Order the result table by `start_id`.

The query result format is in the following example:

```
Logs table:
+------------+
| log_id     |
+------------+
| 1          |
| 2          |
| 3          |
| 7          |
| 8          |
| 10         |
+------------+

Result table:
+------------+--------------+
| start_id   | end_id       |
+------------+--------------+
| 1          | 3            |
| 7          | 8            |
| 10         | 10           |
+------------+--------------+
The result table should contain all ranges in table Logs.
From 1 to 3 is contained in the table.
From 4 to 6 is missing in the table
From 7 to 8 is contained in the table.
Number 9 is missing in the table.
Number 10 is contained in the table.
```



## 题目解读

&emsp;查找连续日志编号的起始与结束编号。

## 程序设计

* 对连续区间编号，并最终统计相同编号的最小、最大值。

```mysql
SELECT
    min(log_id) AS start_id,
    max(log_id) AS end_id
FROM 
    (SELECT 
        log_id,
        CASE WHEN @pre_log = log_id - 1
            THEN @no := @no
            ELSE @no := @no + 1
            END AS no,
        @pre_log := log_id
    FROM Logs, (SELECT @pre_log := 0, @no := 0) v
    ) t
GROUP BY no;
```

## 性能分析

执行用时：298ms，在所有MySQL提交中击败了54.41%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。