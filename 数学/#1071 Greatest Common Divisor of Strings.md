[toc]

For strings `S` and `T`, we say "`T` divides `S`" if and only if `S = T + ... + T`  (`T` concatenated with itself 1 or more times)

Return the largest string `X` such that `X` divides `str1` and `X` divides `str2`.



**Note**:

* $1 \le \text{str1.length} \le 1000$
* $1 \le \text{str2.length} \le 1000$
* `str1[i]` and `str2[i]` are English uppercase letters.



## 题目解读

&emsp;求两个字符串的最大公因子字符串。

```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        
    }
}
```

## 程序设计

* 最基本的思路先求每个字符串的最小公因子字符串，然后比较求出最大字符串。

```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        // 求出两个字符串的最小单元
        int l1 = repeatSubstring(str1), l2 = repeatSubstring(str2);
        if (l1 != l2) return "";
        String sub1 = str1.substring(0, l1), sub2 = str2.substring(0, l2);
        if (!sub1.equals(sub2)) return "";

        // 求最大公约数
        int gcd = gcd(str1.length() / l1, str2.length() / l2);
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < gcd; i++) {
            sb.append(sub1);
        }
        return sb.toString();
    }

    // KMP next数组
    private int repeatSubstring(String s) {
        int[] next = new int[s.length()];
        int k = -1;
        next[0] = k;
        for (int i = 1; i < s.length(); i++) {
            while (k != -1 && s.charAt(k + 1) != s.charAt(i)) {
                k = next[k];
            }
            if (s.charAt(k + 1) == s.charAt(i)) k++;
            next[i] = k;
        }

        // 如果存在重复子字符串，则得到最小单元
        int subLen = s.length() - 1 - next[s.length() - 1];
        if (s.length() % subLen == 0) return subLen;
        else return s.length();
    }

    private int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(N,M))$，空间复杂度为$O(\max(N,M))$。

执行用时：2ms，在所有java提交中击败了28.59%的用户。

内存消耗：39.8MB，在所有java提交中击败了45.66%的用户。

## 官方解题

&emsp;官方采用数学的思路，当两个字符串存在公因子时，其前后拼接必然相等；假设两个字符串长度分别为$n$和$m$，$m \ge n$，如果前后拼接的字符串相等，则表明中间段$m - n$和最后一段$n$（或前面一段$n$）前后拼接相等，问题又变为$m - n$字符串与$n$长度字符串的最大公因子字符串，如此循环直到字符串相等，就是最大公因子。实际上上述过程就是求两个数字最大公因子的辗转相除的过程，从而得到数学方法：

```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (!(str1 + str2).equals(str2 + str1)) return "";
        int gcd = gcd(str1.length(), str2.length());
        return str1.substring(0, gcd);
    }

    private int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}
```