[toc]

Given a sorted integer array **nums**, where the range of elements are in the **inclusive range** `[lower, upper]`, return its missing ranges.



## 题目解读

&emsp;给定有序数组和数据范围，求没有数据的区间。

```java
class Solution {
    public List<String> findMissingRanges(int[] nums, int lower, int upper) {

    }
}
```

## 程序设计

* 使用一个变量记录下一个数字`pre`，如果不存在则说明存在间隙，区间为`pre~num-1`，此处需要判断区间是否只有一个元素；其次需要注意数据溢出的问题。

```java
class Solution {
    public List<String> findMissingRanges(int[] nums, int lower, int upper) {
        List<String> res = new LinkedList<>();

        // 存在数据溢出，故使用长整型
        long pre = lower;
        for (int num : nums) {
            // 存在空隙
            if (pre < num && pre <= upper) {
                // 区间只有一个元素
                if (num - 1 == pre) res.add(Long.toString(pre));
                else res.add(pre + "->" + (num - 1));
            }

            pre = (long)num + 1;
        }
        // 最后判断是否达到上界
        if (pre == upper) res.add(Long.toString(upper));
        else if (pre < upper) res.add(pre + "->" + upper);

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了70.44%的用户。

内存消耗：38.4MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;暂无，密切关注。参考社区思路，直接比较相邻的值来确定区间。

```java
class Solution {
    public List<String> findMissingRanges(int[] nums, int lower, int upper) {
        List<String> res = new LinkedList<>();

        if (nums == null || nums.length == 0) {
            res.add(appendRange(lower, upper));
            return res;
        };
        if (nums[0] > lower) res.add(appendRange(lower, nums[0] - 1));
        for (int i = 0; i < nums.length - 1; i++) {
            // 与后继相等或相连，则无间隙，继续
            if (nums[i] == nums[i + 1] || nums[i] == nums[i + 1] - 1) continue;
            res.add(appendRange(nums[i] + 1, nums[i + 1] - 1));
        }
        if (nums[nums.length - 1] < upper) res.add(appendRange(nums[nums.length - 1] + 1, upper));

        return res;
    }

    private String appendRange(int lower, int upper) {
        if (lower == upper) return Integer.toString(lower);
        else return new StringBuilder(Integer.toString(lower))
                .append("->").append(Integer.toString(upper)).toString();
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了20.00%的用户。