[toc]

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to count the total strobogrammatic numbers that exist in the range of $\text{low} \le \text{num} \le \text{high}$.



**Note**:
Because the range might be a large number, the low and high numbers are represented as string.



## 题目解读

&emsp;寻找区间内的中心对称数个数。

```java
class Solution {
    public int strobogrammaticInRange(String low, String high) {

    }
}
```

## 程序设计

* 为[#247 Strobogrammatic Number II](./#247 Strobogrammatic Number II.md)的进阶，首先如果上下界数字长度不等，可采用数学方式计算上下界长度之间的对称数数目，首位只有$1$、$6$、$8$、$9$四个选择，中间位有五个选择，如果是奇数长度，最中间有$0$、$1$、$8$三个选择。
* 经过上述计算，只剩下计算同等长度上下界之间对称数数目，可采用[#247 Strobogrammatic Number II](./#247 Strobogrammatic Number II.md)回溯思路，需注意每次试探都要判断数字是否超出范围剪枝。

```java
class Solution {
    public int strobogrammaticInRange(String low, String high) {
        int n1 = low.length(), n2 = high.length();
        int res = 0;
        // 不符合要求
        if (n1 > n2) return res;

        // 对于上下界长度之间的值采用排列组合数学计算得到数目
        for (int i = n1 + 1; i < n2; i++) {
            res += strobogrammaticNum(i);
        }
        // 上下界长度相等，回溯计算
        if (n1 == n2) res += countStrobogrammatic(new char[n1], 0, low, high);
        // 上下界长度不等，分别计算
        else {
            res += countStrobogrammatic(new char[n1], 0, low, productNum(n1, true));
            res += countStrobogrammatic(new char[n2], 0, productNum(n2, false), high);
        }
        return res;
    }

    private int strobogrammaticNum(int len) {
        int pre = len / 2, center = len % 2;
        // 开头4个数字和前半部分5个数字的组合
        int res = 4 * (int)Math.pow(5, pre - 1);
        // 中间3个数字组合
        if (center == 1) res *= 3;
        return res;
    }

    private int countStrobogrammatic(char[] strs, int idx, String low, String high) {
        // 结束
        if (idx == (low.length() + 1) / 2) 
            return check(strs, strs.length - 1, low, high) ? 1 : 0;

        int res = 0;
        // 尝试0（不能为开头）
        if (low.length() == 1 || idx != 0) {
            strs[idx] = strs[low.length() - idx - 1] = '0';
            // 检查是否在区间内，并回溯
            if (check(strs, idx, low, high)) res += countStrobogrammatic(strs, idx + 1, low, high);
        }

        // 尝试1、8
        strs[idx] = strs[low.length() - idx - 1] = '1';
        if (check(strs, idx, low, high)) res += countStrobogrammatic(strs, idx + 1, low, high);
        strs[idx] = strs[low.length() - idx - 1] = '8';
        if (check(strs, idx, low, high)) res += countStrobogrammatic(strs, idx + 1, low, high);

        // 尝试6、9
        if (idx != low.length() / 2) {
            strs[idx] = '6';
            strs[low.length() - idx - 1] = '9';
            if (check(strs, idx, low, high)) res += countStrobogrammatic(strs, idx + 1, low, high);
            strs[idx] = '9';
            strs[low.length() - idx - 1] = '6';
            if (check(strs, idx, low, high)) res += countStrobogrammatic(strs, idx + 1, low, high);
        }

        return res;
    }

    // 产生固定长度的最大数或最小数
    private String productNum(int len, boolean bigger) {
        char[] strs = new char[len];
        if (bigger) for (int i = 0; i < len; i++) strs[i] = '9';
        else {
            strs[0] = '1';
            for (int i = 1; i < len; i++) strs[i] = '0';
        }
        return new String(strs);
    }

    // 检查当前数字是否超出范围
    private boolean check(char[] strs, int idx, String low, String high) {
        for (int i = 0; i <= idx; i++) {
            if (strs[i] > low.charAt(i)) break;
            if (strs[i] < low.charAt(i))  return false;
        }
        for (int i = 0; i <= idx; i++) {
            if (strs[i] < high.charAt(i)) break;
            if (strs[i] > high.charAt(i)) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度最坏为$O(5^{\frac{N}{2}})$，空间复杂度为$O(N)$，其中$N$为上下界数字字符串的最大长度。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。