[toc]

Given an array of integers that is already **sorted in ascending order**, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

Note:

* Your returned answers (both index1 and index2) are not zero-based.
* You may assume that each input would have exactly one solution and you may not use the same element twice.



## 题目解读

&emsp;有序数组中查找两个数之和等于给定的数。

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        
    }
}
```

## 程序设计

* 由于是有序数组，从左至右遍历，二分查找另一个值，如果找到就返回，没有找到就更新`right`，即遍历的范围，因为`right`为当前值相加后不超过`target`的位置，当前元素的后继值必然大于等于当前值，`right`后的值相加肯定大于`target`。

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] res = new int[2];
        int index = 0, left, right = numbers.length - 1;
        // 遍历，界限为right
        for(; index < right; index++) {
            int temp = target - numbers[index];
            // 如果当前值和最大值之和小于target，跳过，继续下一轮循环
            if(numbers[index] + numbers[right] < target) {
                continue;
            }
            left = index + 1;
            // 查找，找到就返回，未找到就更新right
            while(left <= right) {
                int mid = (left + right) / 2;
                // 找到并返回
                if(numbers[mid] == temp) {
                    res[0] = index + 1;
                    res[1] = mid + 1;
                    return res;
                }
                // 二分
                if(numbers[mid] < temp) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        // 题目限定必然有一组解，此处不走
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度最好的情况为第一次就查到，$O(\log_2N)$，最坏情况为遍历$N - 1$次，每次查询$O(\log_2N)$，而`right`边界总是定位在$N - 1$的位置上，这样需要$O(N\log_2N)$；空间复杂度为$O(1)$。

执行用时 :3 ms, 在所有 Java 提交中击败了33.33%的用户

内存消耗 :43 MB, 在所有 Java 提交中击败了5.08%的用户

## 官方解题

&emsp;官方采用双指针的算法：

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] res = new int[2];
        int left = 0, right = numbers.length - 1;
        while(left < right) {
            int temp = numbers[left] + numbers[right];
            if(temp == target) {
                res[0] = left + 1;
                res[1] = right + 1;
                return res;
            }
            if(temp > target) {
                right--;
            } else {
                left++;
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了98.94%的用户。

内存消耗：42.9MB，在所有java提交中击败了5.08%的用户。

> 社区有些使用二分查找的方法都是边遍历变二分查找，没有更新边界。