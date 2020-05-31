[toc]

Table: `Transactions`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| country        | varchar |
| state          | enum    |
| amount         | int     |
| trans_date     | date    |
+----------------+---------+
id is the primary key of this table.
The table has information about incoming transactions.
The state column is an enum of type ["approved", "declined"].
```

Table:` Chargebacks`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| trans_id       | int     |
| charge_date    | date    |
+----------------+---------+
Chargebacks contains basic information regarding incoming chargebacks from some transactions placed in Transactions table.
trans_id is a foreign key to the id column of Transactions table.
Each chargeback corresponds to a transaction made previously even if they were not approved.
```



Write an SQL query to find for each month and country, the number of approved transactions and their total amount, the number of chargebacks and their total amount.

Note: In your query, given the month and country, ignore rows with all zeros.

The query result format is in the following example:

```
Transactions table:
+------+---------+----------+--------+------------+
| id   | country | state    | amount | trans_date |
+------+---------+----------+--------+------------+
| 101  | US      | approved | 1000   | 2019-05-18 |
| 102  | US      | declined | 2000   | 2019-05-19 |
| 103  | US      | approved | 3000   | 2019-06-10 |
| 104  | US      | approved | 4000   | 2019-06-13 |
| 105  | US      | approved | 5000   | 2019-06-15 |
+------+---------+----------+--------+------------+

Chargebacks table:
+------------+------------+
| trans_id   | trans_date |
+------------+------------+
| 102        | 2019-05-29 |
| 101        | 2019-06-30 |
| 105        | 2019-09-18 |
+------------+------------+

Result table:
+----------+---------+----------------+-----------------+-------------------+--------------------+
| month    | country | approved_count | approved_amount | chargeback_count  | chargeback_amount  |
+----------+---------+----------------+-----------------+-------------------+--------------------+
| 2019-05  | US      | 1              | 1000            | 1                 | 2000               |
| 2019-06  | US      | 3              | 12000           | 1                 | 1000               |
| 2019-09  | US      | 0              | 0               | 1                 | 5000               |
+----------+---------+----------------+-----------------+-------------------+--------------------+
```



## 题目解读

&emsp;根据国家、月份统计通过、退出的交易。注意通过、退出不是同一维度。

## 程序设计

* 由于通过和退出没有什么直接关联，可以分别查询然后使用`UNION ALL`合并。

```mysql
SELECT 
    c.month AS month,
    c.country AS country,
    sum(CASE c.state WHEN 'approved' THEN 1 ELSE 0 END) AS approved_count,
    sum(CASE c.state WHEN 'approved' THEN c.amount ELSE 0 END) AS approved_amount,
    sum(CASE c.state WHEN 'chargeback' THEN 1 ELSE 0 END) AS chargeback_count,
    sum(CASE c.state WHEN 'chargeback' THEN c.amount ELSE 0 END) AS chargeback_amount
FROM (
    # 通过交易
    (SELECT
        date_format(t.trans_date, '%Y-%m') AS month, 
        t.country AS country,
        t.amount AS amount,
        t.state AS state
    FROM Transactions t WHERE t.state = 'approved') UNION ALL 
    # 退出交易
    (SELECT 
        date_format(c.trans_date, '%Y-%m') AS month, 
        t.country AS country,
        t.amount AS amount,
        'chargeback' AS state
    FROM Transactions t RIGHT JOIN Chargebacks c ON t.id = c.trans_id)
) c GROUP BY c.month, c.country;
```

## 性能分析

执行用时：421ms，在所有MySQL提交中击败了36.20%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。