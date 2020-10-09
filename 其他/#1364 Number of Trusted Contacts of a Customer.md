[toc]

Table: `Customers`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| customer_name | varchar |
| email         | varchar |
+---------------+---------+
customer_id is the primary key for this table.
Each row of this table contains the name and the email of a customer of an online shop.
```


Table: `Contacts`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | id      |
| contact_name  | varchar |
| contact_email | varchar |
+---------------+---------+
(user_id, contact_email) is the primary key for this table.
Each row of this table contains the name and email of one contact of customer with user_id.
This table contains information about people each customer trust. The contact may or may not exist in the Customers table.
```

Table: `Invoices`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| invoice_id   | int     |
| price        | int     |
| user_id      | int     |
+--------------+---------+
invoice_id is the primary key for this table.
Each row of this table indicates that user_id has an invoice with invoice_id and a price.
```




Write an SQL query to find the following for each `invoice_id`:

* `customer_name`: The name of the customer the invoice is related to.
* `price`: The price of the invoice.
* `contacts_cnt`: The number of contacts related to the customer.
* `trusted_contacts_cnt`: The number of contacts related to the customer and at the same time they are customers to the shop. (i.e His/Her email exists in the Customers table.)

Order the result table by `invoice_id`.

The query result format is in the following example:

```
Customers table:
+-------------+---------------+--------------------+
| customer_id | customer_name | email              |
+-------------+---------------+--------------------+
| 1           | Alice         | alice@leetcode.com |
| 2           | Bob           | bob@leetcode.com   |
| 13          | John          | john@leetcode.com  |
| 6           | Alex          | alex@leetcode.com  |
+-------------+---------------+--------------------+
Contacts table:
+-------------+--------------+--------------------+
| user_id     | contact_name | contact_email      |
+-------------+--------------+--------------------+
| 1           | Bob          | bob@leetcode.com   |
| 1           | John         | john@leetcode.com  |
| 1           | Jal          | jal@leetcode.com   |
| 2           | Omar         | omar@leetcode.com  |
| 2           | Meir         | meir@leetcode.com  |
| 6           | Alice        | alice@leetcode.com |
+-------------+--------------+--------------------+
Invoices table:
+------------+-------+---------+
| invoice_id | price | user_id |
+------------+-------+---------+
| 77         | 100   | 1       |
| 88         | 200   | 1       |
| 99         | 300   | 2       |
| 66         | 400   | 2       |
| 55         | 500   | 13      |
| 44         | 60    | 6       |
+------------+-------+---------+
Result table:
+------------+---------------+-------+--------------+----------------------+
| invoice_id | customer_name | price | contacts_cnt | trusted_contacts_cnt |
+------------+---------------+-------+--------------+----------------------+
| 44         | Alex          | 60    | 1            | 1                    |
| 55         | John          | 500   | 0            | 0                    |
| 66         | Bob           | 400   | 2            | 0                    |
| 77         | Alice         | 100   | 3            | 2                    |
| 88         | Alice         | 200   | 3            | 2                    |
| 99         | Bob           | 300   | 2            | 0                    |
+------------+---------------+-------+--------------+----------------------+
Alice has three contacts, two of them are trusted contacts (Bob and John).
Bob has two contacts, none of them is a trusted contact.
Alex has one contact and it is a trusted contact (Alice).
John doesn't have any contacts.
```



## 题目解读

&emsp;查询可信联系人的数量。可信联系人定义为电子邮件存在于客户列表中。

## 程序设计

* 将顾客表与客户表`LEFT JOIN`联表查询。

```mysql
SELECT 
    t1.invoice_id,
    t1.customer_name,
    t1.price,
    ifnull(t2.contacts_cnt, 0) AS contacts_cnt,
    ifnull(t2.trusted_contacts_cnt, 0) AS trusted_contacts_cnt
FROM (
    SELECT 
        i.invoice_id,
        c.customer_id,
        c.customer_name, 
        i.price
    FROM Invoices i, Customers c
    WHERE i.user_id = c.customer_id
) t1 LEFT JOIN (
    SELECT 
        c1.user_id, 
        c1.contact_name,
        count(c1.user_id) AS contacts_cnt,
        sum(CASE WHEN c2.email IS NULL THEN 0 ELSE 1 END) AS trusted_contacts_cnt
    FROM Contacts c1 LEFT JOIN Customers c2 ON c1.contact_email = c2.email
    GROUP BY c1.user_id
) t2 ON t1.customer_id = t2.user_id
ORDER BY t1.invoice_id
;
```

## 性能分析

执行用时：905ms，在所有MySQL提交中击败了87.72%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。