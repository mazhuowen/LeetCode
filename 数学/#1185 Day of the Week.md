[toc]

Given a date, return the corresponding day of the week for that date.

The input is given as three integers representing the `day`, `month` and `year` respectively.

Return the answer as one of the following values `{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}`.



**Constraints:**

- The given dates are valid dates between the years `1971` and `2100`.



## 题目解读

&emsp;给定`1971`到`2100`中的任意时间，计算当天是星期几。

```java
class Solution {
    public String dayOfTheWeek(int day, int month, int year) {

    }
}
```

## 程序设计

* 由于从`1971`年开始，可以计算截止`1971`年的天数，然后取余。需注意闰年的判断，普通闰年能被4整除不能被100整除；世纪闰年可以被400整除，不能漏判。

```java
class Solution {
    // 从1971年1月1日开始，当天是周五，故索引1处是周五
    String[] delta = new String[]{"Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday"};
    int[] monthDay = new int[]{-1, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    public String dayOfTheWeek(int day, int month, int year) {
        if (year > 2100 || year < 1971) throw new IllegalArgumentException("invalid param");

        // 计算天数
        int days = 0;
        for (int y = 1971; y < year; y++) {
            if (isLeap(y)) days += 366;
            else days += 365;
        }
        for (int m = 1; m < month; m++) {
            days += monthDay[m];
            if (m == 2 && isLeap(year)) days++;
        }
        days += day;
        // 取余
        return delta[days % 7];
    }

    private boolean isLeap(int year) {
        return year % 4 == 0 && year % 100 != 0 || year % 400 == 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;暂无，密切关注。