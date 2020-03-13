[toc]

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in $O(n)$ complexity.



## 题目解读

&emsp;找到未排序数组中元素可组成的最大连续值序列。

```java
class Solution {
    public int longestConsecutive(int[] nums) {

    }
}
```

## 程序设计

* 最容易想到的是排序后遍历比较，但需要注意重复值的情况。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums == null || nums.length == 0) {
            return 0;
        }
        // 排序
        Arrays.sort(nums);
        // 记录最大长度
        int maxLen = 1;
        // 记录当前长度
        int curLen = 1;
        // 记录前一个值
        int pre = nums[0];
        for(int i = 1; i < nums.length; i++) {
            // 连续序列
            if(nums[i] == pre + 1) {
                curLen++;
            } 
            // 存在重复值，此时跳过继续遍历
            else if(nums[i] == pre) {
                continue;
            }
            // 不连续，计数重新置为1
            else {
                maxLen = Math.max(maxLen, count);
                count = 1;
            }
            pre = nums[i];
        }
        // 最后还需要比较最后一段连续序列和之前的最大序列
        return Math.max(maxLen, count);
    }
}
```

## 性能分析

&emsp;时间性能为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了99.97%的用户。

内存消耗：39.5MB，在所有java提交中击败了7.89%的用户。

## 官方解题

&emsp;官方解题还提供了哈希表的方法，使用集合记录数字，然后查找不连续的最小数字，从该数出发查询递增序列，并记录长度。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for (int num : nums) {
            set.add(num);
        }

        int longestStreak = 0;

        for (int num : set) {
            // 当前数字是不连续的最小值
            if (!set.contains(num-1)) {
                int currentNum = num;
                int currentStreak = 1;
                // 以该值为起点遍历大的数
                while (set.contains(currentNum+1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }
                // 记录长度
                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}
```

&emsp;虽然两层循环，但只从连续序列的最小值开始遍历，时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了81.10%的用户。

内存消耗：41MB，在所有java提交中击败了5.05%的用户。