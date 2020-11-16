[toc]

Given a time represented in the format `"HH:MM"`, form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.

You may assume the given input string is always valid. For example, `"01:34"`, `"12:09"` are all valid. `"1:34"`, `"12:9"` are all invalid.



## 题目解读

&emsp;给定时间格式字符串，计算使用给定字符串中数字构成的最接近的时间。

```java
class Solution {
    public String nextClosestTime(String time) {

    }
}
```

## 程序设计

* 最低位开始找大于当前值的最小值替换，其后的值填充为最小值。

```java
class Solution {
    public String nextClosestTime(String time) {
        if (time == null || time.length() != 5) throw new IllegalArgumentException("invalid param");

        // 对时间里的数字排序
        char[] times = time.toCharArray();
        int[] record = new int[4];
        record[0] = times[0] - '0'; record[1] = times[1] - '0'; record[2] = times[3] - '0'; record[3] = times[4] - '0';
        Arrays.sort(record);
        
        // 将最低位替换为较大值，得到最进近时间
        int greater = search(record, times[4] - '0');
        if (greater != -1) {
            times[4] = (char)(record[greater] + '0');
            return new String(times);
        }
        // 分钟最高位替换为较大值，低位填充最小值
        greater = search(record, times[3] - '0');
        if (greater != -1 && record[greater] < 6) {
            times[3] = (char)(record[greater] + '0');
            times[4] = (char)(record[0] + '0');
            return new String(times);
        }
         // 小时最低位替换为较大值，低位填充最小值
        greater = search(record, times[1] - '0');
        if (greater != -1 && (times[0] <= '1' || record[greater] < 5)) {
            times[1] = (char)(record[greater] + '0');
            times[3] = (char)(record[0] + '0');
            times[4] = (char)(record[0] + '0');
            return new String(times);
        }
        // 小时最高位替换为较大值，低位填充最小值
        greater = search(record, times[0] - '0');
        if (greater != -1 && record[greater] < 3) {
            times[0] = (char)(record[greater] + '0');
            times[1] = (char)(record[0] + '0');
            times[3] = (char)(record[0] + '0');
            times[4] = (char)(record[0] + '0');
            return new String(times);
        }
        // 如果不存在当前天的最接近时间，则第二天最短时间，即全部填充最小值是最接近的时间
        times[0] = (char)(record[0] + '0');
        times[1] = (char)(record[0] + '0');
        times[3] = (char)(record[0] + '0');
        times[4] = (char)(record[0] + '0');
        return new String(times);
    }

    // 查找大于目标值的最小数字
    private int search(int[] nums, int target) {
        for (int i = 0; i < 4; i++) {
            if (nums[i] > target) return i;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方还提供了尝试所有组合的思路。