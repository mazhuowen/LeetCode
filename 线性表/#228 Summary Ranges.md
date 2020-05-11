[toc]

Given a sorted integer array without duplicates, return the summary of its ranges.



## 题目解读

&emsp;将有序数组中连续序列合并为区间表示。

```java
class Solution {
    public List<String> summaryRanges(int[] nums) {

    }
}
```

## 程序设计

* 连续区间起始结束索引之差等于索引位置数值之差。


```java
class Solution {
    public List<String> summaryRanges(int[] nums) {
        List<String> res = new LinkedList<>();
        if (nums == null || nums.length == 0) return res;

        // 记录起始索引
        int preIdx = -1;
        for (int i = 0; i < nums.length; i++) {
            // 连续，则继续
            if (preIdx != -1 && i - preIdx == nums[i] - nums[preIdx]) continue;

            if (preIdx != -1) {
                // 不连续，则将前面的连续序列拼接为区间表示
                StringBuffer sb = new StringBuffer();
                sb.append(nums[preIdx]);
                // 注意只有一个数的情况
                if (i - 1 > preIdx) sb.append("->").append(nums[i - 1]);
                res.add(sb.toString());
            }
            preIdx = i;
        }
        // 最后一段连续序列
        StringBuffer sb = new StringBuffer();
        sb.append(nums[preIdx]);
        if (nums.length - 1 > preIdx) sb.append("->").append(nums[nums.length - 1]);
        res.add(sb.toString());
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;官方思路类似。