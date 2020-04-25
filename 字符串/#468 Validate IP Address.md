[toc]

Write a function to check whether an input string is a valid IPv4 address or IPv6 address or neither.

**IPv4** addresses are canonically represented in dot-decimal notation, which consists of four decimal numbers, each ranging from 0 to 255, separated by dots ("."), e.g.,`172.16.254.1`;

Besides, leading zeros in the IPv4 is invalid. For example, the address `172.16.254.01` is invalid.

**IPv6** addresses are represented as eight groups of four hexadecimal digits, each group representing 16 bits. The groups are separated by colons (":"). For example, the address `2001:0db8:85a3:0000:0000:8a2e:0370:7334` is a valid one. Also, we could omit some leading zeros among four hexadecimal digits and some low-case characters in the address to upper-case ones, so `2001:db8:85a3:0:0:8A2E:0370:7334` is also a valid IPv6 address(Omit leading zeros and using upper cases).

However, we don't replace a consecutive group of zero value with a single empty group using two consecutive colons (::) to pursue simplicity. For example, `2001:0db8:85a3::8A2E:0370:7334` is an invalid IPv6 address.

Besides, extra leading zeros in the IPv6 is also invalid. For example, the address `02001:0db8:85a3:0000:0000:8a2e:0370:7334` is invalid.

Note: You may assume there is no extra space or special characters in the input string.



## 题目解读

&emsp;检查IP地址是否有效。

```java
class Solution {
    public String validIPAddress(String IP) {
        
    }
}
```

## 程序设计

* 对于IPv4地址，由四部分构成，每部分都是$0 \sim 255$的数，需注意结尾是`.`的情况和数字高位是0的情况；其次需注意数字溢出的情况。
* 对于IPv6地址，由8部分构成，每部分都是不超过16bit位的数，需判断是否超出4位，每一位字符是否是16进制字符；还需注意结尾是`:`的情况。
* 上述两种情况都需注意结尾为分隔符，及中间位置分隔符相连的情况。

```java
class Solution {
    String hexdigits = "0123456789abcdefABCDEF";

    public String validIPAddress(String IP) {
        if (IP == null || IP.isEmpty()) return "Neither";
        String[] ips = IP.split("\\.");
        if (ips.length <= 1) {
            if (IP.charAt(IP.length() - 1) == ':') return "Neither";
            ips = IP.split(":");
            if (isIPv6(ips)) return"IPv6";
        } else {
            if (IP.charAt(IP.length() - 1) == '.') return "Neither";
            if (isIPv4(ips)) return "IPv4";
        }

        return "Neither";
    }

    private boolean isIPv4(String[] ips) {
        if (ips.length != 4) return false;

        for (String ip : ips) {
            int num = 0;
            if (ip.isEmpty() || ip.length() > 3) return false;
            for (int i = 0; i < ip.length(); i++) {
                char cur = ip.charAt(i);
                // 不是数字
                if (!Character.isDigit(cur)) return false;

                num = num * 10 + (cur - '0');
                // 高位存在0
                if (i != 0 && num == (cur - '0')) return false;
            }
            // 大于255
            if (num > 255) return false;
        }
        return true;
    }

    private boolean isIPv6(String[] ips) {
        if (ips.length != 8) return false;
        for (String ip : ips) {
            // 两个:相连的情况
            if (ip.isEmpty()) return false;
            
            // 大于4位，无效
            if (ip.length() > 4) return false;

            // 检查是否是16进制字符
            for (char c : ip.toCharArray()) {
                if (hexdigits.indexOf(c) == -1) return false;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.68%的用户。

内存消耗：37.6MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方提供思路不再阐述。