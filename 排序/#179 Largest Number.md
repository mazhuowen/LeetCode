[toc]

Given a list of non negative integers, arrange them such that they form the largest number.


Note: The result may be very large, so you need to return a string instead of an integer.



## 题目解读

&emsp;给定一组非负整数，重新排列它们的顺序使之组成一个最大的整数。

```java
class Solution {
    public String largestNumber(int[] nums) {

    }
}
```

## 程序设计

* 初步考虑跟基数排序思路很类似，只是比较顺序相反。但这个差距使得程序基本很难修改复用。参考官方解题，采用将数字转化为字符串的方式来比较。每一对数在排序的比较过程中，比较两种连接顺序哪一种更好。
* 最后排好序后优先级高的在最前面。注意排序使用两个字符串的拼接比较，如果只比较单个字符的话，对于`3,30,34`，排序后`34,30,3`，这样`3`和`30`的位置不对。

```java
class Solution {
    public String largestNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++) {
            strs[i] = String.valueOf(nums[i]);
        }
        // 排序（字符串序列大的在前面）
        Arrays.sort(strs, (a, b) -> (b + a).compareTo(a + b));
        StringBuffer sb = new StringBuffer();
        // 最大的字符串是0，则结果是0
        if(strs[0].equals("0")) {
            return "0";
        }
        // 按次序拼接即可
        for(String str : strs) {
            sb.append(str);
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了78.95%的用户。

内存消耗：39MB，在所有java提交中击败了5.04%的用户。

## 官方解题

&emsp;同上。