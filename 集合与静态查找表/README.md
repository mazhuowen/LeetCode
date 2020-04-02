[toc]

## 静态查找表

&emsp;静态查找表与动态查找表相对应，静态查找表用于查询不用于插入删除等，通常使用顺序表存储，查找涉及各种查找算法。对于无序表，只能使用顺序查找；对于有序表可以使用二分查找；对于分布均匀的有序表可以使用插入查找；对于分块分布于内存中的数据可以使用分块查找。

&emsp;静态查找表最常用的是二分查找。原始二分查找形式伪代码如下：

```java
int left = 0, right = len - 1;
while(left <= right) {
    int mid = left + (right - left) / 2;
    if(nums[mid] == target) {
        return mid;
    }
    if(nums[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
return -1;
```

原始二分查找只用于查找，如果没有查找到`target`，整个数组小于`target`，则`right`在`len-1`的位置，而`left`在`len`的位置；如果整个数组大于`target`，则`left`在`0`的位置，`right`在`-1`的位置；除此之外最终`left`会落在大于`target`的第一个数上，`right`会落在小于`target`的第一个数上。利用二分查找的基本形式可以扩展出一系列变形，如查找存在重复数的有序数组的界限。通过改变循环内的`left`、`right`定位逻辑和外层循环的条件来达成不同的目的。

&emsp;设计二分查找的变形形式时，需要注意`right=mid-1`和`right=mid`意义的不同，`mid-1`抛弃了`mid`，而`right=mid`包含了`mid`，根据`mid`含义和目标的不同做取舍。还要尽量避免`left=mid`的操作，容易造成死循环。最后外层循环条件`left<=right`和`left<right`也会因为任务的不用而不同，需要具体分析。

### 顺序查找



### 二分查找

* $\clubs$中等[#33 Search in Rotated Sorted Array](./#33 Search in Rotated Sorted Array.md)    两段有序数组拼接的二分查找
* $\bigstar$中等[#34 Find First and Last Position of Element in Sorted Array](./#34 Find First and Last Position of Element in Sorted Array.md)    二分查找区间
* 简单[#35 Search Insert Position](./#35 Search Insert Position.md)    [#34 Find First and Last Position of Element in Sorted Array](./#34 Find First and Last Position of Element in Sorted Array.md) 的延续，左边界为插入位置
* 中等[#74 Search a 2D Matrix](./#74 Search a 2D Matrix.md)    递增矩形的二分查找
* 中等[#81 Search in Rotated Sorted Array II](./#81 Search in Rotated Sorted Array II.md)    [#33 Search in Rotated Sorted Array](./#33 Search in Rotated Sorted Array.md)存在重复数的扩展
* 中等[#153 Find Minimum in Rotated Sorted Array](./#153 Find Minimum in Rotated Sorted Array.md)     [#33 Search in Rotated Sorted Array](./#33 Search in Rotated Sorted Array.md)的变形
* $\clubs$困难[#154 Find Minimum in Rotated Sorted Array II](./#154 Find Minimum in Rotated Sorted Array II.md)    [#153 Find Minimum in Rotated Sorted Array](./#153 Find Minimum in Rotated Sorted Array.md)的延续
* $\clubs$中等[#162 Find Peak Element](./#162 Find Peak Element.md)    无序有规律数组的二分查找问题
* 简单[#167 Two Sum II - Input array is sorted](./#167 Two Sum II - Input array is sorted.md)    双指针，可用二分法解答
* $\bigstar$中等[#275 H-Index II](./#275 H-Index II.md)    二分查找变形应用
* 简单[#278 First Bad Version](./#278 First Bad Version.md)    二分查找的数据溢出问题
* $\bigstar$中等[#287 Find the Duplicate Number](./#287 Find the Duplicate Number.md)    对整数范围的二分查找（非最优解）
* $\clubs$困难[#302 Smallest Rectangle Enclosing Black Pixels](./#302 Smallest Rectangle Enclosing Black Pixels.md)    二分查找区间
* 简单[#374 Guess Number Higher or Lower](./#374 Guess Number Higher or Lower.md)    二分查找基础应用
* 中等[#436 Find Right Interval](./#436 Find Right Interval.md)    排序后利用二分查找
* 简单[#441 Arranging Coins](./#441 Arranging Coins.md)    数学规律总结，二分查找可实现
* 中等[#475 Heaters](./#475 Heaters.md)    排序后使用二分查找
* $\bigstar$困难[#644 Maximum Average Subarray II](./#644 Maximum Average Subarray II.md)    二分查找猜测平均值并验证
* 中等[#668 Kth Smallest Number in Multiplication Table](./#668 Kth Smallest Number in Multiplication Table.md)    二分缩小统计范围
* 简单[#702 Search in a Sorted Array of Unknown Size](./#702 Search in a Sorted Array of Unknown Size.md)    基本二分查找
* 简单[#704 Binary Search](./#704 Binary Search.md)    二分查找
* $\bigstar$困难[#710 Random Pick with Blacklist](./#710 Random Pick with Blacklist.md)    二分查找连续数值中的缺失值
* $\bigstar$中等[#719 Find K-th Smallest Pair Distance](./#719 Find K-th Smallest Pair Distance.md)    对距离范围二分查找统计
* 中等[#875 Koko Eating Bananas](./#875 Koko Eating Bananas.md)    利用二分查找遍历判断
* $\clubs$中等[#1060 Missing Element in Sorted Array](./#1060 Missing Element in Sorted Array.md)    查找第k个值问题

### 插入查找



### 分块查找



