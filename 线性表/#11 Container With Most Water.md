[toc]

Given n non-negative integers $a_1, a_2, \dots, a_n$ , where each represents a point at coordinate $(i, a_i)$. n vertical lines are drawn such that the two endpoints of line i is at $(i, a_i)$ and $(i, 0)$. Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.



## 题目解读

&emsp;给定数组，数值代表挡板的高度，索引代表挡板的位置，需求解两个挡板使得积水最大。

```java
class Solution {
    public int maxArea(int[] height) {
        
    }
}
```

## 程序设计

* 考虑到积水问题由两个挡板中较低的那个决定，可以初始化两个挡板为起始位置和结束位置，计算其积水面积；然后从两个挡板中较低的那个开始迭代，凡是迭代到的小于等于低挡板高度的挡板，其面积必然小于之前的值；直到遇到比较低挡板高的挡板，计算面积，重复以上过程。
* 注意分析示例，面积计算是`low*(highIndex-lowIndex)`，而不是`low*(highIndex-lowIndex - 1)`。

```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        // 双指针，记录左右挡板位置
        int leftMax = 0, rightMax = height.length - 1;
        while(leftMax < rightMax) {
            // 较小的挡板
            int minBoun = Math.min(height[leftMax], height[rightMax]);
            // 求积水量
            maxArea = Math.max(maxArea, minBoun * (rightMax - leftMax));
            
            // 左挡板较小，向右遍历
            if(height[leftMax] < height[rightMax]) {
                // 当前挡板比左挡板还低，积水必定更小，继续迭代
                while(++leftMax < rightMax && height[leftMax] <= minBoun) {
                }
            }  
            // 右挡板比较小，向左遍历
            else {
                // 当前挡板比右挡板还低，积水必然更小，继续迭代
                while(--rightMax > leftMax && height[rightMax] <= minBoun) {
                }
            }
        }
        return maxArea;
    }
}
```

测试样例：`[1,8,6,2,5,4,8,3,7]`输出49。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：2ms，在所有java提交中击败了99.63%的用户。

内存消耗：40.2MB，在所有java提交中击败了12.81%的用户。

## 官方解题

&emsp;思路一致。