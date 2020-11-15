[toc]

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.



**Note**:

* The length of both `num1` and `num2` is < 110.
* Both `num1` and `num2` contain only digits `0-9`.
* Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
* You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.



## 题目解读

&emsp;字符串乘法。

```java
class Solution {
    public String multiply(String num1, String num2) {

    }
}
```

## 程序设计

* 最基本的思路为将数字2的每一位与数字1相乘，然后相加。

```java
class Solution {
    public String multiply(String num1, String num2) {
        if ("0".equals(num1) || "0".equals(num2)) return "0";

        String res = "0";
        for (int i = 0; i < num2.length(); i++) {
            // 每一位相乘
            String prod = multiply(num1, num2.charAt(i) - '0', num2.length() - 1 - i);
            // 相加
            res = add(res, prod);
        }
        return res;
    }

    private String multiply(String num1, int num2, int offset) {
        StringBuffer sb = new StringBuffer();
        // 进位
        int r = 0;
        for (int i = num1.length() - 1; i >= 0; i--) {
            int result = (num1.charAt(i) - '0') * num2 + r;
            sb.insert(0, result % 10);
            r = result / 10;
        }
        if (r > 0) sb.insert(0, r);
        while (offset > 0) {
            sb.append("0");
            offset--;
        }
        return sb.toString();
    }

    private String add(String num1, String num2) {
        StringBuffer sb = new StringBuffer();
        int idx = 0, r = 0;
        int m = num1.length(), n = num2.length();
        while (idx < m && idx < n) {
            int result = (num1.charAt(m - idx - 1) - '0') + (num2.charAt(n - idx - 1) - '0') + r;
            sb.insert(0, result % 10);
            r = result / 10;
            idx++;
        }
        while (idx < n) {
            int result = (num2.charAt(n - idx - 1) - '0') + r;
            sb.insert(0, result % 10);
            r = result / 10;
            idx++;
        }
        while (idx < m) {
            int result = (num1.charAt(m - idx - 1) - '0') + r;
            sb.insert(0, result % 10);
            r = result / 10;
            idx++;
        }
        if (r > 0) {
            sb.insert(0, r);
        }
        return sb.toString();
    }
}
```

* 上述形式还可以继续优化，即可以从数字2高位乘起，这样相加会少低位的大量的0，加快速度，相应的加法需要增加偏移参数。但一种更好的思路是，数字1的第$i$位和数字2的$j$为相乘，结果必然参与到了结果的$i + j$位，只需要取各位，进位加到$i + j + 1$位去即可。

```java
class Solution {
    public String multiply(String num1, String num2) {
        if ("0".equals(num1) || "0".equals(num2)) return "0";

        char[] a = num1.toCharArray(), b = num2.toCharArray();
        int[] prod = new int[a.length + b.length];

        // 注意数组高位是数字低位
        for (int i = a.length - 1; i >= 0; i--) {
            int n1 = a[i] - '0';
            for (int j = b.length - 1; j >= 0; j--) {
                int n2 = b[j] - '0';
                int res = prod[i + j + 1] + n1 * n2;
                // 当前为
                prod[i + j + 1] = res % 10;
                // 进位
                prod[i + j] += res / 10;

            }
        }

        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < prod.length; i++) {
            // 最高位为0，跳过
            if (i == 0 && prod[i] == 0) continue;
            sb.append(prod[i]);
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(N+M)$。

执行用时：59ms，在所有java提交中击败了9.02%的用户。

内存消耗：40.1MB，在所有java提交中击败了6.10%的用户。

&emsp;优化后时间复杂度为$O(NM)$，空间复杂度为$O(N + M)$。

执行用时：5ms，在所有java提交中击败了77.34%的用户。

内存消耗：39.5MB，在所有java提交中击败了6.10%的用户。

## 官方解题

&emsp;暂无，密切关注。