[toc]



```mysql
DELETE p1 FROM Person p1, Person p2 WHERE p1.Id > p2.Id AND p1.Email = p2.Email;
```



执行用时 :1630 ms, 在所有 MySQL 提交中击败了12.38%的用户

内存消耗 :0B, 在所有 MySQL 提交中击败了100.00%的用户