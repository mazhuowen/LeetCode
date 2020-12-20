[toc]

Given a string `date` representing a Gregorian calendar date formatted as `YYYY-MM-DD`, return the day number of the year.

 

**Example 1**:

```
Input: date = "2019-01-09"
Output: 9
Explanation: Given date is the 9th day of the year in 2019.
```

**Example 2**:

```
Input: date = "2019-02-10"
Output: 41
```

**Example 3**:

```
Input: date = "2003-03-01"
Output: 60
```

**Example 4**:

```
Input: date = "2004-03-01"
Output: 61
```



**Constraints**:

* $\text{date.length} == 10$
* $\text{date[4]} == \text{date[7]} == \text{'-'}$, and all other `date[i]`'s are digits
* `date` represents a calendar date between Jan 1st, 1900 and Dec 31, 2019.



## 题目解读

&emsp;判断是当前年的第几天。

```java
class Solution {
    public int dayOfYear(String date) {

    }
}
```

## 程序设计

* 判断闰年并计算。

```java
class Solution {
    private int[] monthDay = new int[]{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    public int dayOfYear(String date) {
        String[] dates = date.split("-");
        int year = Integer.parseInt(dates[0]), month = Integer.parseInt(dates[1]), day = Integer.parseInt(dates[2]);
        boolean flag = year % 4 == 0 && year % 100 != 0 || year % 400 == 0;

        int res = 0;
        // 此处可提前计算好截止m个月的天数
        for (int m = 1; m < month; m++) {
            res += monthDay[m - 1];
            if (m == 2 && flag) res++;
        }
        res += day;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：2 ms, 在所有 Java 提交中击败了81.59%的用户。

内存消耗：36.7 MB, 在所有 Java 提交中击败了63.06%的用户。

## 官方解题

&emsp;暂无，密切关注。
