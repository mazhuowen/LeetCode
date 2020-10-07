[toc]

Table: `Users`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| user_id      | int     |
| user_name    | varchar |
| credit       | int     |
+--------------+---------+
user_id is the primary key for this table.
Each row of this table contains the current credit information for each user.
```

Table: `Transactions`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| trans_id      | int     |
| paid_by       | int     |
| paid_to       | int     |
| amount        | int     |
| transacted_on | date    |
+---------------+---------+
trans_id is the primary key for this table.
Each row of this table contains the information about the transaction in the bank.
User with id (paid_by) transfer money to user with id (paid_to).
```


Leetcode Bank (LCB) helps its coders in making virtual payments. Our bank records all transactions in the table Transaction, we want to find out the current balance of all users and check wheter they have breached their credit limit (If their current credit is less than 0).

Write an SQL query to report.

* `user_id`
* `user_name`
* `credit`, current balance after performing transactions.  
* `credit_limit_breached`, check credit_limit ("Yes" or "No")

Return the result table in **any** order.

The query result format is in the following example.

 ```
Users table:
+------------+--------------+-------------+
| user_id    | user_name    | credit      |
+------------+--------------+-------------+
| 1          | Moustafa     | 100         |
| 2          | Jonathan     | 200         |
| 3          | Winston      | 10000       |
| 4          | Luis         | 800         | 
+------------+--------------+-------------+

Transactions table:
+------------+------------+------------+----------+---------------+
| trans_id   | paid_by    | paid_to    | amount   | transacted_on |
+------------+------------+------------+----------+---------------+
| 1          | 1          | 3          | 400      | 2020-08-01    |
| 2          | 3          | 2          | 500      | 2020-08-02    |
| 3          | 2          | 1          | 200      | 2020-08-03    |
+------------+------------+------------+----------+---------------+

Result table:
+------------+------------+------------+-----------------------+
| user_id    | user_name  | credit     | credit_limit_breached |
+------------+------------+------------+-----------------------+
| 1          | Moustafa   | -100       | Yes                   | 
| 2          | Jonathan   | 500        | No                    |
| 3          | Winston    | 9900       | No                    |
| 4          | Luis       | 800        | No                    |
+------------+------------+------------+-----------------------+
Moustafa paid $400 on "2020-08-01" and received $200 on "2020-08-03", credit (100 -400 +200) = -$100
Jonathan received $500 on "2020-08-02" and paid $200 on "2020-08-08", credit (200 +500 -200) = $500
Winston received $400 on "2020-08-01" and paid $500 on "2020-08-03", credit (10000 +400 -500) = $9990
Luis didn't received any transfer, credit = $800
 ```



## 题目解读

&emsp;查询信用卡是否透支。

## 程序设计

* 分别统计支出和支入，然后对比总额度。

```mysql
SELECT 
	a.user_id AS USER_ID, 
	a.user_name AS USER_NAME, 
	a.credit + a.amount - b.amount AS CREDIT, 
	CASE WHEN a.credit + a.amount >= b.amount THEN 'No' ELSE 'Yes' END as CREDIT_LIMIT_BREACHED 
FROM 
    (
        SELECT u.user_id, u.user_name, u.credit, sum(ifnull(t.amount, 0)) AS amount FROM Users u LEFT JOIN Transactions t 
        ON u.user_id = t.paid_to GROUP BY u.user_id
    ) a,
    (
        SELECT u.user_id, u.user_name, u.credit, sum(ifnull(t.amount, 0)) AS amount FROM Users u LEFT JOIN Transactions t 
        ON u.user_id = t.paid_by GROUP BY u.user_id
    ) b
WHERE a.user_id = b.user_id; 
```

## 性能分析

执行用时：454ms，在所有MySQL提交中击败了86.25%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用先`UNION ALL`然后再统计的方式。

```mysql
SELECT t.user_id, Users.user_name, sum(t.credit) AS credit, (CASE WHEN sum(t.credit) < 0 THEN 'Yes' ELSE 'No' END) AS credit_limit_breached
FROM 
    (
        SELECT paid_by AS user_id, sum(amount)*(-1) AS credit FROM Transactions GROUP BY paid_by
        UNION ALL
        SELECT paid_to AS user_id, sum(amount) AS credit FROM Transactions GROUP BY paid_to
        UNION ALL
        SELECT user_id, credit FROM Users
    ) t
LEFT JOIN Users ON t.user_id = Users.user_id
GROUP BY t.user_id, Users.user_name
```

