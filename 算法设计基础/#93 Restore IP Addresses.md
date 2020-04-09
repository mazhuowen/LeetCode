[toc]

Given a string containing only digits, restore it by returning all possible valid IP address combinations.



## 题目解读

&emsp;给定字符串只包含数字，给出可以组成的`IP`地址。

> 题目描述没有透露`IP`地址必须包含所有的数字，其次根据`IP`地址规则，有四个区间，每个区块数字不能超过255.

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {

    }
}
```

## 程序设计

* 通过回溯尝试所有组合，难点在于回溯判断。首先每个区块数字不能超过255，超过255就不能回溯；其次`IP`中不存在除了0本身之外以0开头的值，如`001`，这需要判断当前数字是否等于当前字符，是则说明之前的位数字是0；最后区间必须是4个，不能超过4个。

```java
class Solution {
    List<String> res;

    public List<String> restoreIpAddresses(String s) {
        res = new LinkedList<>();
        if (s == null || s.length() < 4 || s.length() > 12) return res;
        restoreIpAddresses(s.toCharArray(), 0, new ArrayList<>(), 4);
        return res;
    }

    // start记录回溯地址，count记录剩余的区块数
    private void restoreIpAddresses(char[] s, int start, List<String> ip, int count) {
        // 找到要求的解
        if (start >= s.length && count == 0) {
            // 拼接加入
            StringBuffer sb = new StringBuffer();
            for (String t : ip) {
                sb.append(t).append(".");
            }
            sb.deleteCharAt(sb.length() - 1);
            res.add(sb.toString());
            return;
        }
        // 已遍历完区块不够或区块达到要求还未遍历完，无效
        if (start >= s.length || count == 0) return;

        int num = 0;
        StringBuffer sb = new StringBuffer();
        
        for (int i = start; i < start + 3 && i < s.length; i++) {
            num = num * 10 + (s[i] - '0');
            // 每个ip不能超过三位255，其次不能是以0开头的值
            if (num > 255 || (i > start && num == s[i] - '0')) break;
            sb.append(s[i]);
            
            // 试探
            ip.add(sb.toString());
            restoreIpAddresses(s, i + 1, ip, count - 1);
            // 回溯
            ip.remove(ip.size() - 1);
        }
    }
}
```

## 性能分析

&emsp;由于长度不超过12，时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了97.76%的用户。

内存消耗：39.9MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;思路为回溯，形式较复杂。