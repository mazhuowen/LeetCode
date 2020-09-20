[toc]

Given an array `nums` of n integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.



## 题目解读

&emsp;找到最接近`target`的三数之和。题目没有说明数组不合法时候的返回值，即默认数组长度都是大于等于3的。

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        
    }
}
```

## 程序设计

* 参考[#15 3Sum](./#15 3Sum.md)的思路，先排序，再进行比较。注意此题不要求重复值，不需要去重。

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        // 三数之和与target的差
        int diff = Integer.MAX_VALUE;
        // 遍历，寻找另外两个数使得与i位置的数之和相等
        for(int i = 0; i < nums.length - 2; i++) {
            int low = i + 1;
            int high = nums.length - 1;
            while(low < high) {
                int temp = nums[i] + nums[low] + nums[high];
                // 相等，则是最接近的
                if(temp == target) {
                    return target;
                } 
                // 比target小，需要增大low
                else if(temp < target) {
                    low++;
                }
                // 比target大，需要减少high
                else {
                    high--;
                }
                // 更新diff
                if(Math.abs(target - temp) < Math.abs(diff)) {
                    diff = temp - target;
                }
            }
        }
        return target + diff;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了86.39%的用户。

内存消耗：36.1MB，在所有java提交中击败了85.68%的用户。

## 官方解题

&emsp;暂无，密切关注。