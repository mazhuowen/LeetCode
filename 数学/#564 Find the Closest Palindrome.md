[toc]

Given an integer $n$, find the closest integer (not including itself), which is a palindrome.

The 'closest' is defined as absolute difference minimized between two integers.



**Note**:

* The input $n$ is a positive integer represented by string, whose length will not exceed 18.
* If there is a tie, return the smaller one as answer.



## 题目解读

&emsp;给定数字字符串，找出最接近的回文数字字符串，接近的定义是两个数字之差绝对值最小。如果有多个，返回最小的那个。

```java
class Solution {
    public String nearestPalindromic(String n) {

    }
}
```

## 程序设计

* 参考官方解题思路，首先原字符串镜像为回文串，将高位复制到低位，明显比将低位复制到高位的代价小；以`132954`为例。镜像回文串为`132231`，可见很接近最佳答案`133331`，在得到镜像字符串后可以求出其上一个回文串`133331`和下一个回文串`132231`，通过比对这三个值得到答案。
* 因此奇数长字符串只需考虑中间字符，偶数长需考虑中间两个字符；需要注意当中间字符是$0$或$9$时需要继续遍历到第一个非$0$或非$9$的数字，然后进行相关操作。其次如果操作后得到的结果高位为$0$，则不合法，需特殊处理。

```java
class Solution {
    public String nearestPalindromic(String n) {
        String res;
        // 计算原字符串的镜像字符串
        String p = toPalindromic(n.toCharArray());
        long diff1 = Math.abs(Long.parseLong(n) - Long.parseLong(p));
        // 得到的镜像字符串和原串相等，不是答案，重置绝对值
        if (diff1 == 0) diff1 = Long.MAX_VALUE;
        res = p;

        // 计算比原字符串小的下一个回文字符串
        String le = nextPalindromic(p.toCharArray(), true);
        long diff2 = Math.abs(Long.parseLong(n) - Long.parseLong(le));
        if (diff2 == 0) diff2 = Long.MAX_VALUE;
        // 如果与原串绝对值更小或绝对值相等，当前值更小
        if (diff2  < diff1 || (diff2 == diff1 && Long.parseLong(le) < Long.parseLong(res))) {
            diff1 = diff2;
            res = le;
        }

        // 计算比原字符串大的下一个回文字符串
        String ge = nextPalindromic(p.toCharArray(), false);
        diff2 = Math.abs(Long.parseLong(n) - Long.parseLong(ge));
        if (diff2 == 0) diff2 = Long.MAX_VALUE;
        // 如果与原串绝对值更小或绝对值相等，当前值更小
        if (diff2  < diff1 || (diff2 == diff1 && Long.parseLong(ge) < Long.parseLong(res))) {
            res = ge;
        }
        return res;
    }

    // 找到比当前字符串小或大的下一个回文串
    private String nextPalindromic(char[] strs, boolean flag) {
        // 奇数为中心点，偶数为中心两个位置中的第一个，还需减一
        int idx = strs.length / 2 - (strs.length % 2 == 0 ? 1 : 0);
        int num = strs[idx] - '0';
        // 求下一个小的回文数
        if (flag) {
            // 中心数字大于0，直接减一
            if (num > 0) {
                strs[idx] = (char)(num - 1 + '0');
                if (strs.length % 2 == 0) strs[idx + 1] = strs[idx];
                // 减少后，最高位为0，则此时较小的回文数为长度减少，值为9的字符串
                if (strs[0] == '0') return Integer.toString((int)Math.pow(10, strs.length - 1) - 1);
            } 
            // 中心数字为0
            else {
                // 将与中心相连的0变为9
                while (idx >= 0 && strs[idx] == '0') {
                    strs[idx] = strs[strs.length - 1 - idx] = '9';
                    idx--;
                }
                // 全部为0，此时下一小的回文串为长度减一的全9子串
                // 注意原先长度为1，则下一小的字符串为-1
                if (idx < 0) {
                    return Integer.toString((int)Math.pow(10, strs.length - 1) - 1);
                }
                // 与中心相连的第一个非0值减一
                else {
                    strs[idx]--;
                    strs[strs.length - 1 - idx]--;
                    // 减少后，最高位为0，则此时较小的回文数为长度减少，值为9的字符串
                    if (strs[0] == '0') return Integer.toString((int)Math.pow(10, strs.length - 1) - 1);
                }
            }
        } 
        // 求下一个较大的回文数字
        else {
            // 值小于9，直接加一
            if (num < 9) {
                strs[idx] = (char)(num + 1 + '0');
                if (strs.length % 2 == 0) strs[idx + 1] = strs[idx];
            } 
            // 值大于9
            else {
                // 与中心相连的9替换为0
                while (idx >= 0 && strs[idx] == '9') {
                    strs[idx] = strs[strs.length - 1 - idx] = '0';
                    idx--;
                }
                // 若全是9，则较大的回文数是长度加一的10……01格式数字
                if (idx < 0) {
                    return Integer.toString((int)Math.pow(10, strs.length) + 1);
                } 
                // 与中心相连的第一个非9值加一
                else {
                    strs[idx]++;
                    strs[strs.length - 1 - idx]++;
                }
            }
        }
        return new String(strs);
    }

    // 通过原串的镜像形成回文串
    private String toPalindromic(char[] strs) {
        StringBuffer sb = new StringBuffer();
        int start = strs.length / 2;
        // 如果是奇数，拼接中间字符
        if (strs.length % 2 == 1) sb.append(strs[start]);
        // 镜像拼接
        start--;
        while (start >= 0) {
            sb.append(strs[start]);
            sb.insert(0, strs[start]);
            start--;
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.8MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;上述思路参考官方解题。