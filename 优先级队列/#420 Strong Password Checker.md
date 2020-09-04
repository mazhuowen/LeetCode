[toc]

A password is considered strong if below conditions are all met:

* It has at least 6 characters and at most 20 characters.
* It must contain at least one lowercase letter, at least one uppercase letter, and at least one digit.
* It must NOT contain three repeating characters in a row (`"...aaa..."` is weak, but `"...aa...a..."` is strong, assuming other conditions are met).

Write a function `strongPasswordChecker(s)`, that takes a string s as input, and return the **MINIMUM** change required to make `s` a strong password. If s is already strong, return $0$.

Insertion, deletion or replace of any one character are all considered as one change.



## 题目解读

&emsp;给定字符串，返回使之变为强密码的最少改动次数。

```java
class Solution {
    public int strongPasswordChecker(String s) {

    }
}
```

## 程序设计

* 首先对于连续字符串，如`aaaaa`，替换代价最低，只需替换依次即可，如`aabaa`；其次插入代价较低，需要加入两次，如`aabaab`；最后删除代价最高，需要删除三次，如`aa`。这样，在长度小于等于$20$的情况下优先替换重复字符串，长度不够则需要插入，长度超出，则选哟删除。

* 分析所有情况，假设统计缺失`need`个字符：

  * 字符串长度小于$6$：

    * 不存在连续重复超过$3$的字符，则只需在任意位置插入`need`个字符，并凑够长度为$6$；
    * 存在连续重复超过$3$的字符，如果`need`超过一个，则只需将一个`need`字符插入中间，并拼接剩余的`need-1`个字符，凑够长度为$6$；如果`need`为$0$，则只需将凑够$6$个长度的字符插入中间即可；

    总的情况即`max(6-len,need)`；

  * 字符串长度在$6 \sim 20$，则只需替换连续超过$3$个相等的字符，首先使用`need`字符替换，如果不够则需要额外引入字符；如果剩余，则需要替换完剩余的`need`字符；

  * 对于长度超过$20$的字符：

    * 首先要删除长度到$20$个，假设连续长度超过$3$的子字符串长度为$l$，则$l\ \%\ 3 == 0$的字符串优先删除，因为删除后替换的字符就会减少一个，如$6$个相等字符串，需要插入两个字符，但是删除一个后，只需插入一个字符。这样可使用优先级队列动态比较$l\ \%\ 3$；
    * 如果删除后长度还是大于$20$个，此时已经不存在连续超过$3$的字符串，则删除剩余的长度直到到达$20$，并替换`need`个字符，结束；
    * 如果删除后小于等于$20$，此时需要继续替换连续字符串，直到替换结束，此时如果`need`有剩余，则需要继续替换`need`次。

```java
class Solution {
    public int strongPasswordChecker(String s) {
        if (s == null || s.isEmpty()) return 6;
        if (s.length() < 6) return passwordLessThan6(s);
        else if (s.length() < 20) return passwordBetween6And20(s);
        else return passwordMoreThan20(s);
    }

    private int count(String s, PriorityQueue<Integer> queue) {
        // 小写字母、大写字符、数字的统计数目
        int low = 0, up = 0, digit = 0;
        int len = 0;
        char pre = ' ';
        for (char c : s.toCharArray()) {
            if (Character.isLowerCase(c)) low++;
            else if (Character.isUpperCase(c)) up++;
            else if (Character.isDigit(c)) digit++;

            if (c == pre) len++;
            else {
                if (len >= 3) queue.add(len);
                len = 1;
            }
            pre = c;
        }
        if (len >= 3) queue.add(len);
        return (low == 0 ? 1 : 0) + (up == 0 ? 1 : 0) + (digit == 0 ? 1 : 0);
    }

    private int passwordLessThan6(String s) {
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        int need = count(s, queue);
        // 对于少于6个的字符串，分以下几种情况：
        // 1、不存在重复字符，则只需插入需要的字符数并凑够或超过6个
        // 2、存在重复字符，最多为5个，若need不为0，则只需插入到中间即可，若need为0，则只需插入任意一个不相等字符到中间即可，并凑够6个
        return Math.max(6 - s.length(), need);
    }

    private int passwordBetween6And20(String s) {
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        int need = count(s, queue);
        int num = 0;
        // 对于在6~20间的字符串，将连续字符每隔两个替换第三个为其他字符
        while (!queue.isEmpty()) {
            // 需要的替换长度
            int insert = queue.poll() / 3;
            num += insert;
            // 可将need中的数目作为替换字符
            need = need <= insert ? 0 : need - insert;
        }
        // 返回替换结束及剩余的所需字符之和
        return num + need;
    }

    private int passwordMoreThan20(String s) {
        PriorityQueue<Integer> queue = new PriorityQueue<>((a, b) -> a % 3 - b % 3);
        int need = count(s, queue);
        int num = 0, len = s.length();
        // 删除操作
        while (!queue.isEmpty() && len > 20) {
            // 删除一位
            int cur = queue.poll();
            num++;
            len--;
            if (--cur >= 3) queue.add(cur);
        }
        // 删除后长度仍然大于20
        if (len > 20) return num + len - 20 + need;
        // 替换操作
        while (!queue.isEmpty()) {
            // 需要的替换长度
            int insert = queue.poll() / 3;
            num += insert;
            // 可将need中的数目作为替换字符
            need = need <= insert ? 0 : need - insert;
        }
        // 如果最后还有需要的字符，则需要替换need次
        return num + need;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\frac{N}{3}\log_2\frac{N}{3})$，空间复杂度为$O(\frac{N}{3})$。

执行用时：1ms，在所有java提交中击败了63.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了7.89%的用户。

## 官方解题

&emsp;暂无，密切关注。