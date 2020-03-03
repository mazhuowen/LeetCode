[toc]

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that `nums[i] = nums[j]` and the **absolute** difference between $i$ and $j$ is at most $k$.



## 题目解读

&emsp;判断数组中是否存在相对位置不超过$k$的重复数。

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {

    }
}
```

## 程序设计

* 采用字典记录最后一次出现的数字的下标索引。

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        if(nums == null || nums.length == 0 || k <= 0) {
            return false;
        }
        Map<Integer, Integer> record = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            // 第一次出现
            if(record.get(nums[i]) == null) {
                record.put(nums[i], i);
            } else {
                // 距离小于等于k，返回
                if(i - record.get(nums[i]) <= k) {
                    return true;
                } 
                // 距离大于k，更新最新索引
                else {
                    record.put(nums[i], i);
                }
            }
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了95.98%的用户。

内存消耗：44.7MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;同上。