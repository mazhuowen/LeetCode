[toc]

We can rotate digits by 180 degrees to form new digits. When `0, 1, 6, 8, 9` are rotated 180 degrees, they become `0, 1, 9, 8, 6` respectively. When `2, 3, 4, 5` and `7` are rotated 180 degrees, they become invalid.

A confusing number is a number that when rotated 180 degrees becomes a **different** number with each digit valid.(Note that the rotated number can be greater than the original number.)

Given a positive integer `N`, return the number of confusing numbers between `1` and `N` inclusive.

**Note:**$1 \le N \le 10^9$



## 题目解读

&emsp;易混淆数指的是一个数字在整体旋转180°以后，能够得到一个和原来不同的数，且新数字的每一位都应该是有效的。统计这样的数字个数。

```java
class Solution {
    public int confusingNumberII(int N) {

    }
}
```

## 程序设计

* 由于易混淆数只有0、1、6、8、9构成，且开头不能为0，旋转后数字不能相同，可用回溯试探。
* 问题的关键在于易混淆数的判断和试探过程中边界情况1_000_000_000和整数溢出的处理。

```java
class Solution {
    int count = 0;
    char[] first = new char[]{'1', '6', '8', '9'};
    char[] ele = new char[]{'0', '1', '6', '8', '9'};

    public int confusingNumberII(int N) {
        if (N <= 1) throw new IllegalArgumentException("invalid param");

        // 根据题目数字不超过10位
        char[] curNum = new char[10];
        int idx = 0;
        for (char num : first) {
            curNum[idx++] = num;
            confusingNumberII(curNum, num - '0', idx--, N);
        }

        return count;
    }

    // curNum为数字的字符数组形式，方便判断易混淆数；num为数字形式，方便判断判断
    private void confusingNumberII(char[] curNum, long num, int idx, int limit) {
        // 超出限制（包括数字溢出为负的情况）
        if (num < 0 || num > limit) return;
        // 统计
        if (isConfusing(curNum, idx)) count++;
        // 达到最大位数，不再回溯（不写在上面是因为类似1_000_000_000这样的边界数字也是易混淆数，不统计会漏掉）
        if (idx == curNum.length) return;

        for (char n : ele) {
            curNum[idx++] = n;
            confusingNumberII(curNum, num * 10 + (n - '0'), idx--, limit);
        }
    }

    // 易混淆数判断函数
    private boolean isConfusing(char[] curNum, int idx) {
        int left = 0, right = idx - 1;
        while (left <= right) {
            char c = curNum[left++];
            switch (c) {
                case '6':
                    if (curNum[right--] != '9') return true;
                    break;
                case '9':
                    if (curNum[right--] != '6') return true;
                    break;
                default:
                    if (curNum[right--] != c) return true;
                    break;
            }
        }
        // 旋转后与本身相同
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M5^M)$，空间复杂度为$O(M)$，其中$M$为数字$N$的位数。

执行用时：88ms，在所有java提交中击败了81.58%的用户。

内存消耗：36.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。