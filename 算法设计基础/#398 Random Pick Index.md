[toc]

Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.



Note:
The array size can be very large. Solution that uses too much extra space will not pass the judge.



## 题目解读

&emsp;给定一个可能含有重复元素的整数数组，要求随机输出给定的数字的索引。给定的数字一定存在于数组中。

```java
class Solution {

    public Solution(int[] nums) {

    }
    
    public int pick(int target) {

    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int param_1 = obj.pick(target);
 */
```

## 程序设计

* 题目要求用最少的空间，采用储水池抽样，$k = 1$。

```java
class Solution {
    Random random;
    int[] nums;

    public Solution(int[] nums) {
        this.random = new Random();
        this.nums = nums;
    }
    
    public int pick(int target) {
        int res = -1;
        // 模拟遍历数组索引（全部是target的数组）
        int idx = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != target) continue;

            // 第一个目标数，放入池子
            if (res == -1) {
                idx++;
                res = i;
            }
            // 生成索引比较替换
            else {
                idx++;
                // 替换
                if (random.nextInt(idx) == 0) res = i;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：66ms，在所有java提交中击败了93.46%的用户。

内存消耗：47.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。