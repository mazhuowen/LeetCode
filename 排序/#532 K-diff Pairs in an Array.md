[toc]

Given an array of integers and an integer **k**, you need to find the number of **unique** k-diff pairs in the array. Here a **k-diff** pair is defined as an integer pair `(i, j)`, where `i` and `j` are both numbers in the array and their **absolute difference** is k.



Note:

* The pairs `(i, j)` and `(j, i)` count as the same pair.
* The length of the array won't exceed `10000`.
* All the integers in the given input belong to the range: `[-1e7, 1e7]`.



## 题目解读

&emsp;查找数组中差值为`k`的组合数。

```java
class Solution {
    public int findPairs(int[] nums, int k) {

    }
}
```

## 程序设计

* 对数组排序，然后双指针遍历比较。

```java
class Solution {
    public int findPairs(int[] nums, int k) {
        if (nums == null || nums.length <= 1 || k < 0) return 0;

        // 排序
        Arrays.sort(nums);
        
        int count = 0;
        int first = 0, second = 0;
        while (first < nums.length && second < nums.length) {
            // 由于不能使用同一个位置数字，重定向
            if (first >= second) second = first + 1;
            // 遍历到差值大于等于k的第一个位置
            while (second < nums.length && nums[second] - nums[first] < k) {
                second++;
            }
            // 判断，如果符合条件则计数
            if (second < nums.length && second != first && nums[second] - nums[first] == k) count++;
            
            // 遍历跳过相同值，避免重复
            while (first + 1 < nums.length && nums[first] == nums[first + 1]) first++;
            // 迭代下一个位置
            first++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：5ms，在所有java提交中击败了95.42%的用户。

内存消耗：39.4MB，在所有java提交中击败了85.71%的用户。

## 官方解题

&emsp;暂无，密切关注。