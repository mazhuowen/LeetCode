[toc]

Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.



Note: Your algorithm should run in $O(n)$ time complexity and $O(1)$ space complexity.



## 题目解读

&emsp;设计迭代器打印二维数组，注意数组元素长度不等。

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {

    }
}
```

## 程序设计

* 最长递增子序列的简化，只需记录两个数。

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if (nums == null || nums.length < 3) return false;

        Integer v1 = null, v2 = null;
        for (int num : nums) {
            if (v1 == null || v1 >= num) v1 = num;
            else if (v2 == null || v2 >= num) v2 = num;
            else return true;
        }

        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了52.43%的用户。

内存消耗：39.8MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;同上。