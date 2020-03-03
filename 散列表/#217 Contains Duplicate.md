[toc]

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.



## 题目解读

&emsp;判断数组是否存在重复数字。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {

    }
}
```

## 程序设计

* 最简单的想法是维护一个集合，遍历时判断是否在集合中。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        if(nums == null || nums.length == 0) {
            return false;
        }
        Set<Integer> set = new HashSet<>();
        for(int num : nums) {
            if(set.contains(num)) {
                return true;
            }
            set.add(num);
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了71.62%的用户。

内存消耗：45.7MB，在所有java提交中击败了5.05%的用户。

## 官方解题

&emsp;官方除了上述思路，还有排序后遍历的思路。