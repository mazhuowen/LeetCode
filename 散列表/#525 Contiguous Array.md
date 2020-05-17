[toc]

Given a binary array, find the maximum length of a contiguous subarray with equal number of `0` and `1`.



**Note:** The length of the given binary array will not exceed `50,000`.



## 题目解读

&emsp;计算子数组中`0`与`1`数目相等的最长长度。

```java
class Solution {
    public int findMaxLength(int[] nums) {

    }
}
```

## 程序设计

* 每次遍历遇到`0`则减一，遇到`1`则加一，每次判断之前是否也存在相等的值，存在说明这之间的序列是平衡的。

```java
class Solution {
    public int findMaxLength(int[] nums) {
        if (nums == null || nums.length <= 1) return 0;
        int maxLen = 0, level = 0;
        Map<Integer, Integer> couner = new HashMap<>();
        // 很重要
        couner.put(0, -1);

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) level--;
            else level++;

            // 只维护第一个level的起始索引，这样得到的序列最长
            if (couner.containsKey(level)) maxLen = Math.max(maxLen, i - couner.get(level));
            else couner.put(level, i);
        }

        return maxLen;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：29ms，在所有java提交中击败了49.72%的用户。

内存消耗：49.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方同上。社区时间性能较好的方法把哈希表换为数组。