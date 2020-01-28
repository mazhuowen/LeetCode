[toc]

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the same element twice.



## 题目解读

&emsp;给定数组和数值，返回数组中两个元素之和等于该数值的下标。题目要求只需返回一组解；同时不能使用相同元素两次，需向面试官询问这里的相同元素是指下标相同的元素还是指值相同的元素，其次需要注意不满足情况的话，是返回`null`还是空表。数据结构为数组。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        
    }
}
```

## 程序设计

* 最容易想到的，遍历顺序表，在当前结点遍历其后的结点是否满足要求，然后遍历下个结点，重复操作。这种思路需要两层循环实现，外层遍历顺序表，内层遍历当前结点后的结点，时间复杂度是$O(N^2)$。
* 要减少时间复杂度，必须引入额外空间。首先考虑到引入字典，键保存元素值，值保存对应的下标。这样只需要一次遍历。每遍历到一个结点值为`val`，去字典查对应的`target - val`是否存在，存在则返回，不存在则保存当前结点键值对。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> indexRecord = new HashMap<>(nums.length);
        // 遍历
        for(int i = 0; i < nums.length; i++) {
            // 如果存在对应结点，返回
            if(indexRecord.get(target - nums[i]) != null) {
                return  new int[]{indexRecord.get(target - nums[i]), i};
            } else {
                // 更新字典
                indexRecord.put(nums[i], i);
            }
        }
        // 未找到，返回空表
        return new int[]{};
    }
}
```

测试样例：`[2, 7, 11, 15]`，`target = 9`，返回`[0, 1]`；`target = 10`返回`[]`。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：2ms，在所有java提交中击败了99.46%的用户。

内存消耗：38.1MB，在所有java提交中击败了11.77%的用户。

## 官方解题

&emsp;官方思路同上。