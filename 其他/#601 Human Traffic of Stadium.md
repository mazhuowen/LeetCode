[toc]

X city built a new stadium, each day many people visit it and the stats are saved as these columns: **id**, **visit_date**, **people**

Please write a query to display the records which have 3 or more consecutive rows and the amount of people more than 100(inclusive).

For example, the table `stadium`:

```
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 1    | 2017-01-01 | 10        |
| 2    | 2017-01-02 | 109       |
| 3    | 2017-01-03 | 150       |
| 4    | 2017-01-04 | 99        |
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-08 | 188       |
+------+------------+-----------+
```

For the sample data above, the output is:

```
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-08 | 188       |
+------+------------+-----------+
```



**Note**:
Each day only have one row record, and the dates are increasing with id increasing.



## 题目解读

&emsp;查询连续三天以上人流量大于100的记录。

## 程序设计

* 参考官方解题，采用自连接过滤并查询表`s1`中天数分别在第一个、第二个、第三个的数据，注意连续超过三天的都可以拆解为三天。

```mysql
SELECT DISTINCT s1.*
FROM stadium s1, stadium s2, stadium s3
WHERE s1.people >= 100 AND s2.people >= 100 AND s3.people >= 100
AND (
    (s2.id - s1.id = 1 AND s3.id - s2.id = 1)
    OR
    (s1.id - s2.id = 1 AND s3.id - s1.id = 1)
    OR
    (s2.id - s3.id = 1 AND s1.id - s2.id = 1)
)
ORDER BY s1.id
;
```

## 性能分析

执行用时：251ms，在所有MySQL提交中击败了41.68%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;见上。