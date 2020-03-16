[toc]

## 优先级队列

&emsp;优先级队列主要用作动态排序。题目中纯二叉堆的应用主要有最小堆top-k问题，在这个基础上衍生出过滤频率、计数等进行排序的问题；除了最基本的应用，还有基于堆的扩展问题，如丑数求解等；剩下的一些问题需要非常深入的理解，堆往往是解决的一种思想，如[#621 Task Scheduler](./#621 Task Scheduler.md) 、[#407 Trapping Rain Water II](./#407 Trapping Rain Water II.md)等，需要理解和扩展思路。

### 二叉堆

* 中等[#215 Kth Largest Element in an Array](./#215 Kth Largest Element in an Array.md)    top-k问题堆的应用
* $\clubs$中等[#253 Meeting Rooms II](./#253 Meeting Rooms II.md)    会议结束时间做优先级队列
* $\bigstar$困难[#621 Task Scheduler](./#621 Task Scheduler.md)    根据任务数入队判断
* $\clubs$中等[#295 Find Median from Data Stream](./#295 Find Median from Data Stream.md)    双堆保存前后数字判断中位数
* 中等[#347 Top K Frequent Elements](./#347 Top K Frequent Elements.md)    最小堆实现top-k的问题
* 繁杂[#355 Design Twitter](./#355 Design Twitter.md)    top-k问题
* 困难[#358 Rearrange String k Distance Apart](./#358 Rearrange String k Distance Apart.md)    [#621 Task Scheduler](./#621 Task Scheduler.md)的延续
* $\clubs$中等[#373 Find K Pairs with Smallest Sums](./#373 Find K Pairs with Smallest Sums.md)    最小堆top-k问题
* 中等[#378 Kth Smallest Element in a Sorted Matrix](./#378 Kth Smallest Element in a Sorted Matrix.md)    [#373 Find K Pairs with Smallest Sums](./#373 Find K Pairs with Smallest Sums.md)的延续
* $\bigstar$困难[#407 Trapping Rain Water II](./#407 Trapping Rain Water II.md)    通过围墙入最小堆迭代计算
* 简单[#451 Sort Characters By Frequency](./#451 Sort Characters By Frequency.md)    统计频率top-k的基本应用
* 繁杂[#502 IPO](./#502 IPO.md)    双堆，题目和常理不符，理解是关键
* 中等[#692 Top K Frequent Words](./#692 Top K Frequent Words.md)    最小堆的top-k问题
* 简单[#703 Kth Largest Element in a Stream](./#703 Kth Largest Element in a Stream.md)    最小堆top-k问题
* $\clubs$中等[#759 Employee Free Time](./#759 Employee Free Time.md)    堆在时间区间的应用
* 中等[#767 Reorganize String](./#767 Reorganize String.md)    [#621 Task Scheduler](./#621 Task Scheduler.md)思路的简化
* $\clubs$中等[#778 Swim in Rising Water](./#778 Swim in Rising Water.md)    同[#407 Trapping Rain Water II](./#407 Trapping Rain Water II.md)思想，将边界入堆
* $\clubs$困难[#786 K-th Smallest Prime Fraction](./#786 K-th Smallest Prime Fraction.md)    扩展堆及二分查找的应用
* $\bigstar$困难[#857 Minimum Cost to Hire K Workers](./#857 Minimum Cost to Hire K Workers.md)    巧妙使用薪资效率比排序然后用堆筛选

### 最小最大堆



### 归并堆



## 应用

### 最小语言集

* $\clubs$中等[#264 Ugly Number II](./#264 Ugly Number II.md)    堆的拓展入队，可用动态规划优化
* 中等[#313 Super Ugly Number](./#313 Super Ugly Number.md)    [#264 Ugly Number II](./#264 Ugly Number II.md)的延续

