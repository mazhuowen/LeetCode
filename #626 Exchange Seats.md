[toc]

Mary is a teacher in a middle school and she has a table `seat` storing students' names and their corresponding seat ids.

The column **id** is continuous increment.


Mary wants to change seats for the adjacent students.


Can you write a SQL query to output the result for Mary?

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

For the sample input, the output is:

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```



Note:
If the number of students is odd, there is no need to change the last one's seat.



## 题目解读

&emsp;交换相邻的学生的座位。若有奇数个，最后一位不交换。

## 程序设计

* 使用`CASE WHEN`语句，对奇数`id`加一，偶数`id`减一。需要统计最大`id`比较

```mysql
SELECT 
    (CASE 
        WHEN mod(s.id, 2) = 0 THEN id - 1
        WHEN mod(s.id, 2) != 0 AND id != max THEN id + 1 
        ELSE id 
    END) AS id,
    s.student
FROM
    seat s,
    (
    SELECT 
        max(id) as max
    FROM 
        seat 
    ) t
ORDER BY id
```

## 性能分析

执行用时 :217 ms, 在所有 MySQL 提交中击败了39.71%的用户

内存消耗 :0B, 在所有 MySQL 提交中击败了100.00%的用户

## 官方解题

&emsp;