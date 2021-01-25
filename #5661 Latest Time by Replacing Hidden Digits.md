[toc]

You are given a string `time` in the form of `hh:mm`, where some of the digits in the string are hidden (represented by `?`).

The valid times are those inclusively between `00:00` and `23:59`.

Return the latest valid time you can get from `time` by replacing the hidden digits.

 

**Example 1**:

```
Input: time = "2?:?0"
Output: "23:50"
Explanation: The latest hour beginning with the digit '2' is 23 and the latest minute ending with the digit '0' is 50.
```

**Example 2**:

```
Input: time = "0?:3?"
Output: "09:39"
```

**Example 3**:

```
Input: time = "1?:22"
Output: "19:22"
```



**Constraints**:

* `time` is in the format `hh:mm`.
* It is guaranteed that you can produce a valid time from the given string.



## 题目解读

&emsp;给定字符串，求可以表示的最大时间。

```java
class Solution {
    public String maximumTime(String time) {

    }
}
```

## 程序设计

* 模拟判断即可。

```java
class Solution {
    public String maximumTime(String time) {
        char[] seq = time.toCharArray();
        for (int i = 0; i < seq.length; i++) {
            if (seq[i] != '?') continue;
            // 小时第一位，需要根据第二位判断
            if (i == 0) {
                if (seq[i + 1] == '?' || seq[i + 1] <= '3') seq[i] = '2';
                else seq[i] = '1';
            } 
            // 小时第二位，需要根据第一位判断
            else if (i == 1) {
                if (seq[i - 1] == '2') seq[i] = '3';
                else seq[i] = '9';
            } 
            // 分钟
            else if (i == 3) {
                seq[i] = '5';
            } 
            else {
                seq[i] = '9';
            }
        }
        return new String(seq);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。



## 官方解题

&emsp;

```java

```

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。
