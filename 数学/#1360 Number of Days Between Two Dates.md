[toc]

Write a program to count the number of days between two dates.

The two dates are given as strings, their format is `YYYY-MM-DD` as shown in the examples.



**Constraints:**

- The given dates are valid dates between the years `1971` and `2100`.



## 题目解读

&emsp;计算两个日期的相差天数。

```java
class Solution {
    public int daysBetweenDates(String date1, String date2) {

    }
}
```

## 程序设计

* 转化为天，求差值。需注意闰年。

```java
class Solution {
    int[] months = new int[]{-1, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    public int daysBetweenDates(String date1, String date2) {
        return Math.abs(transDateToInt(date1) - transDateToInt(date2));
    }

    private int transDateToInt(String date) {
        String[] dates = date.split("-");
        int year = Integer.valueOf(dates[0]);
        int month = Integer.valueOf(dates[1]);
        int day = Integer.valueOf(dates[2]);

        int res = 0;
        for (int y = 1971; y < year; y++) {
            if ((y % 4 == 0 && y % 100 != 0) || y % 400 == 0) res += 366;
            else res += 365;
        }

        for (int m = 1; m < month; m++) {
            res += months[m];
            if (m == 2 && ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0)) res++;
        }
        res += day;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了85.64%的用户。

内存消耗：37.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。