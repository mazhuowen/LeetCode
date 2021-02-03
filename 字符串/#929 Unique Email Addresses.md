[toc]

Every email consists of a local name and a domain name, separated by the @ sign.

For example, in `alice@leetcode.com`, `alice` is the local name, and `leetcode.com` is the domain name.

Besides lowercase letters, these emails may contain `'.'`s or `'+'`s.

If you add periods (`'.'`) between some characters in the **local name** part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, `"alice.z@leetcode.com"` and `"alicez@leetcode.com"` forward to the same email address.  (Note that this rule does not apply for domain names.)

If you add a plus (`'+'`) in the **local name**, everything after the first plus sign will be **ignored**. This allows certain emails to be filtered, for example `m.y+name@email.com` will be forwarded to `my@email.com`.  (Again, this rule does not apply for domain names.)

It is possible to use both of these rules at the same time.

Given a list of `emails`, we send one email to each address in the list.  How many different addresses actually receive mails? 

 

**Example 1**:

```
Input: ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
Output: 2
Explanation: "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails
```



**Note**:

* $1 \le \text{emails[i].length} \le 100$
* $1 \le \text{emails.length} \le 100$
* Each `emails[i]` contains exactly one `'@'` character.
* All local and domain names are non-empty.
* Local names do not start with a `'+'` character.



## 题目解读

&emsp;邮箱地址域名前的点好可以省略，加号与@间的字符可删除，求最终的邮件地址数目。

```java
class Solution {
    public int numUniqueEmails(String[] emails) {

    }
}
```

## 程序设计

* 注意@后的点号不能删除，可遍历的时候设置标识来判断是否需要删除。

```java
class Solution {
    public int numUniqueEmails(String[] emails) {
        Set<String> set = new HashSet<>();
            set.add(getEmail(email));
        }
        return set.size();
    }

    private String getEmail(String source) {
        int len = 0;
        char[] seq = new char[source.length()];
        // 加号后的字符、点号是否丢弃
        boolean flag = false, dot = true;
        for (char c : source.toCharArray()) {
            // @前的.和加号后@前的字符都可跳过
            if (dot && c == '.' || flag && c != '@') continue;
            if (c == '+') {
                flag = true;
                continue;
            }
            if (c == '@') {
                flag = false;
                dot = false;
            }
            seq[len++] = c;
        }
        return new String(seq, 0, len);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5 ms, 在所有 Java 提交中击败了99.86%的用户。

内存消耗：38.4 MB, 在所有 Java 提交中击败了94.81%的用户。

## 官方解题

&emsp;官方采用分段处理的思路，将@前第一个加号前的数据单独处理，@后的数据单独处理，逻辑更清晰。
