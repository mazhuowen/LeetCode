[toc]

Compare two version numbers `version1` and `version2`.
If `version1` > `version2` return `1`; if `version1` < `version2` return `-1`;otherwise return `0`.

You may assume that the version strings are non-empty and contain only digits and the `.` character.

The `.` character does not represent a decimal point and is used to separate number sequences.

For instance, `2.5` is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

You may assume the default revision number for each level of a version number to be `0`. For example, version number `3.4` has a revision number of `3` and `4` for its first and second level revision number. Its third and fourth level revision number are both `0`.



Note:

* Version strings are composed of numeric strings separated by dots `.` and this numeric strings may have leading zeroes.
* Version strings do not start or end with dots, and they will not be two consecutive dots.



## 题目解读

&emsp;比较版本号。

```java
class Solution {
    public int compareVersion(String version1, String version2) {

    }
}
```

## 程序设计



```java
class Solution {
    public int compareVersion(String version1, String version2) {
        // 需要转义
        String[] str1 = version1.split("\\."), str2 = version2.split("\\.");

        int idx = 0;
        while (idx < str1.length && idx < str2.length) {
            int num1 = getInteger(str1[idx]), num2 = getInteger(str2[idx]);
            if (num1 > num2) return 1;
            else if (num1 < num2) return -1;
            idx++;
        }
        // 一个已经匹配完，剩余的部分必须是0才会相等
        while (idx < str1.length) {
            if (getInteger(str1[idx++]) != 0) return 1;
        }
        while (idx < str2.length) {
            if (getInteger(str2[idx++]) != 0) return -1; 
        }

        return 0;
    }

    private int getInteger(String s) {
        int num = 0;
        for (char c : s.toCharArray()) {
            num = num * 10 + (c - '0');
        }
        return num;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M,N))$，空间复杂度为$O(\max(M,N))$。

执行用时：1ms，在所有java提交中击败了94.75%的用户。

内存消耗：37.9MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;官方还提供了根据字符逐个比较计算数字的方法，思路一致。