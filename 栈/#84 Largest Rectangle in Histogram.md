[toc]

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.



## 题目解读

&emsp;给定直方图，求解最大的矩形。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        
    }
}
```

## 程序设计

* 最直接的，使用暴力方法遍历计算面积。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        // 依次遍历
        for(int i = 0; i < heights.length; i ++) {
            int minBoun = Integer.MAX_VALUE;
            // 遍历当前元素后的最低bar，并计算面积
            for(int j = i; j < heights.length; j++) {
                minBoun = Math.min(minBoun, heights[j]);
                // 更新面积
                maxArea = Math.max(maxArea, minBoun * (j - i + 1));
            }
        }
        return maxArea;
    }
}
```

测试样例：输入`[2,1,5,6,2,3]`，输出`10`。

## 性能分析

&emsp;时间复杂度$O(N^2)$，空间复杂度$O(1)$。

执行用时：607ms，在所有java提交中击败了17.13%的用户。

内存消耗：41MB，在所有java提交中击败了74.34%的用户。

## 官方解题

&emsp;除了上述的暴力法，官方还提供了分治法。分治法每次寻找最低的bar，计算最低bar为边长的矩形面积，并和左右两边的最大矩形面积比较。其核心思想是当前挡板所能延伸的的最大面积在其两侧的矩形区域即其本身延伸的矩形区域之中的最大者。

<img src="/project/LeetCode/images/#84_1.png"  />

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        return calculateArea(heights, 0, heights.length - 1);
    }

    // 分治法计算矩形面积
    private int  calculateArea(int[] heights, int start, int end) {
        // 递归终止条件
        if(start > end) {
            return 0;
        }
        int minIndex = start;
        // 查找
        for(int i = start; i <= end; i++) {
            if(heights[i] < heights[minIndex]) {
                minIndex = i;
            }
        }
        // 计算
        int curArea = heights[minIndex] * (end - start + 1);
        // 分治递归
        int   leftArea = calculateArea(heights, start, minIndex - 1);
        int rightArea = calculateArea(heights, minIndex + 1, end);
        return Math.max(curArea, Math.max(leftArea, rightArea));
    }
}
```

&emsp;由于分治，总共切分$\log_2N$次，每次方法调用都要遍历`start`到`end`的链表，即$O(N)$，总的平均时间复杂度为$O(N\log_2N)$，空间复杂度由于递归为$O(N)$。

执行用时：409ms，在所有java提交中击败了30.36%的用户。

内存消耗：42.4MB，在所有java提交中击败了9.33%的用户。

&emsp;最优算法利用栈的数据结构。栈中存储挡板索引，当前挡板大于栈顶挡板高度，则入栈；若小于等于栈顶挡板，则出栈计算矩形面积，最后将这个挡板入栈，继续遍历，直到遍历结束。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        for(int i = 0; i < heights.length; i++) {
            // 入栈
            if(stack.peek() == -1 || heights[i] > heights[stack.peek()]) {
                stack.push(i);
            } 
            // 计算面积
            else {
                // 入到小的挡板，出栈计算之前的挡板面积
                while(stack.peek() != -1 && heights[i] <= heights[stack.peek()]) {
                    // i - stack.peek() - 1得到栈中bar的数目
                    // stack中预先入栈-1，防止此处peek操作报空
                    maxArea = Math.max(maxArea, heights[stack.pop()] * (i - stack.peek() - 1));
                }
                stack.push(i);
            }
        }
        // 最后栈中剩下的挡板都是递增顺序，要么是尾部的连续挡板，要么是开头或中间非连续挡板，其它部分挡板都比它搞，导致没有出栈，计算面积，以尾部位置为基准（因为数组中其它位置都比栈中挡板高，栈中挡板可以连到尾部形成矩形区域）
        while(stack.peek() != -1) {
            maxArea = Math.max(maxArea, heights[stack.pop()] * (heights.length - stack.peek() - 1));
        }
        return maxArea;
    }
}
```

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：30ms，在所有java提交中击败了44.87%的用户。

内存消耗：41.6MB，在所有java提交中击败了37.07%的用户。

> 实际上思想是分治法的延续，找到较矮的挡板，则最大矩形是这个挡板两侧所能延续的最大面积；每次遍历遇到较小的挡板就开始出栈计算面积，实际就是计算该较低挡板左侧的矩形面积，其右侧矩形面积在碰到下个较低挡板时开始计算。而较低挡板自身延续的面积会在后续遇到更低挡板或最后计算。例如如下直方图，假设当前结点是2，则出栈计算其左侧最大矩形，即由5、6组成的区域；等到下次计算其右侧区域；在此图中，最后栈中剩余1、2、3，计算3就是计算2的右侧区域，等2出栈就是计算2所能延伸的区域，即从5到3的挡板。
>
> 通过上面分析可知，每次计算都是较低挡板索引减去当前栈中挡板的前一个挡板的索引减1，即$heights[stack.pop()] * (i - stack.peek() - 1))$，其中peek就是pop的前一个挡板索引。需考虑到特殊情况，比如栈空了，以下图中的1为例，此时$i - stack.peek() - 1$会报错，为了处理这种情况，上面算法中栈会预先放入-1，这样挡板1到前面的挡板数$2 - (-1) -1 = 2$，计算出挡板的面积为2。

<img src="/project/LeetCode/images/#84.png"  />