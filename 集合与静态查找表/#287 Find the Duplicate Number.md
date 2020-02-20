[toc]

Given an array `nums` containing $n + 1$ integers where each integer is between 1 and $n$ (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Note:

* You must not modify the array (assume the array is read only).
* You must use only constant, $O(1)$ extra space.
* Your runtime complexity should be less than $O(n^2)$.
* There is only one duplicate number in the array, but it could be repeated more than once.



## 题目解读

&emsp;给定长度为$n + 1$的数组，里面有至多$n$个数，且只有一个数重复最多$n$次，其它数都是唯一的。需要找到重复的数。题目限定时间复杂度和空间复杂度，且不能改变数组结构。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        
    }
}
```

## 程序设计

* 最基础的，两层循环遍历遍历数组，时间复杂度为$O(N^2)$，显然不符合要求。$N + 1$个数，每个数范围为$1 \sim N$，如果某个数字$M$不重复，则小于等于$M$的数的个数必然小于等于$M$，否则$M$之前必然存在重复的数。根据这个思路可以将$1 \sim N$的范围二分查找统计是否重复，并缩小范围。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        // 1～N的范围开始二分
        int left = 1, right = nums.length - 1;
        // left== right就是重复的数
        while(left < right) {
            int mid = left + (right - left) / 2;
            // 统计小于等于mid的数目
            int count = 0;
            for(int num : nums) {
                if(num <= mid) {
                    count++;
                }
            }
            // left到mid间存在重复数
            if(count > mid) {
                right = mid;
            } 
            // left到mid间不存在重复数，则在mid+1到right间必然存在重复数
            else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了61.76%的用户。

内存消耗：43.2MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;官方首先给出了一些常规解，如排序载统计、引入集合等。官方给出的最优解法为弗洛伊德的乌龟和兔子循环检测解法。数组中有$n+1$个数，每个数在$1\sim n$之间，有重复数意味着以数组中的数字作为索引访问数组，存在环。问题转变为环与链表相交点判断的问题。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        // 快慢指针
        int tortoise = nums[0];
        int hare = nums[0];
        // 快慢指针相遇在环中某一点
        do {
            tortoise = nums[tortoise];
            hare = nums[nums[hare]];
        } while (tortoise != hare);

        // 从该点及开始位置同时遍历，相遇位置就是环与链表的交点，也就是重复值
        int ptr1 = nums[0];
        int ptr2 = tortoise;
        while (ptr1 != ptr2) {
            ptr1 = nums[ptr1];
            ptr2 = nums[ptr2];
        }

        return ptr1;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.9MB，在所有java提交中击败了5.03%的用户。