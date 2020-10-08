[toc]

## 队列基本操作

&emsp;队列作为非常常用的数据结构，单独考察队列的题目很少，主要考察循环队列及其队列的深度应用。队列分队首固定的顺序队列、队首不固定的顺序队列及其改进循环队列、链接队列；通常除了循环队列，考题中不需要设计队列，直接使用语言中的集合包。需要理解队列的先进先出思想，结合题目发掘队列的出队、入队规则。循环队列需要注意的是`front`为标志位，需要占用一位，后继是队首，`rear`为队尾标识，指向队尾，特别需要注意队列判空和判满的条件。

* 简单[#3 Longest Substring Without Repeating Characters](./#3 Longest Substring Without Repeating Characters.md)    队列的基本应用
* $\clubs$中等​[#209 Minimum Size Subarray Sum](./#209 Minimum Size Subarray Sum.md)    队列双指针思想，可用二分法实现
* 简单[#225 Implement Stack using Queues](./#225 Implement Stack using Queues.md)    队列原理
* $\bigstar$困难[#239 Sliding Window Maximum](./#239 Sliding Window Maximum.md)    单调递减双向队列
* $\clubs$困难[#340 Longest Substring with At Most K Distinct Characters](./#340 Longest Substring with At Most K Distinct Characters.md)    [#3 Longest Substring Without Repeating Characters](./#3 Longest Substring Without Repeating Characters.md)和[#159 Longest Substring with At Most Two Distinct Characters](../散列表/#159 Longest Substring with At Most Two Distinct Characters.md)的进阶
* 简单[#346 Moving Average from Data Stream](./#346 Moving Average from Data Stream.md)    循环队列
* $\clubs$中等[#353 Design Snake Game](./#353 Design Snake Game.md)    队列模拟贪吃蛇的移动进食
* 简单[#362 Design Hit Counter](./#362 Design Hit Counter.md)    队列用于计时
* $\clubs$中等[#424 Longest Repeating Character Replacement](./#424 Longest Repeating Character Replacement.md)    队列记录主体字符数
* 中等[#582 Kill Process](./#582 Kill Process.md)    树的广度优先搜索
* 简单[#622 Design Circular Queue](./#622 Design Circular Queue.md)    设计循环队列
* 中等[#641 Design Circular Deque](./#641 Design Circular Deque.md)    设计双向循环队列
* $\clubs$困难[#683 K Empty Slots](./#683 K Empty Slots.md)    转化数组线性扫描
* $\bigstar$困难[#862 Shortest Subarray with Sum at Least K](./#862 Shortest Subarray with Sum at Least K.md)    双向队列结合前缀和
* 中等[#904 Fruit Into Baskets](./#904 Fruit Into Baskets.md)    双指针模拟队列
* 简单[#933 Number of Recent Calls](./#933 Number of Recent Calls.md)    队列基本操作
* $\clubs$中等[#950 Reveal Cards In Increasing Order](./#950 Reveal Cards In Increasing Order.md)    队列模拟牌的顺序
* $\clubs$中等[#992 Subarrays with K Different Integers](./#992 Subarrays with K Different Integers.md)    队列统计数字种数
* 中等[#1004 Max Consecutive Ones III](./#1004 Max Consecutive Ones III.md)    队列出队入队关系
* $\bigstar$中等[#1031 Maximum Sum of Two Non-Overlapping Subarrays](./#1031 Maximum Sum of Two Non-Overlapping Subarrays.md)    两个不重叠窗口最大值
* 中等[#1151 Minimum Swaps to Group All 1's Together](./#1151 Minimum Swaps to Group All 1's Together.md)    交换数字与窗口的关系
* 繁杂[#1156 Swap For Longest Repeated Character Substring](./#1156 Swap For Longest Repeated Character Substring.md)    可替换单字符最大重复子串
* 简单[#1180 Count Substrings with Only One Distinct Letter](./#1180 Count Substrings with Only One Distinct Letter.md)    滑动窗口统计字符串
* 中等[#1358 Number of Substrings Containing All Three Characters](./#1358 Number of Substrings Containing All Three Characters.md)    每个位置结尾的最短子串
* 中等[#1567 Maximum Length of Subarray With Positive Product](./#1567 Maximum Length of Subarray With Positive Product.md)    队列统计负数个数
* 中等[#1578 Minimum Deletion Cost to Avoid Repeating Letters](./#1578 Minimum Deletion Cost to Avoid Repeating Letters.md)    队列为相等字符的子字符串
* 中等[#1604 Alert Using Same Key-Card Three or More Times in a One Hour Period](./#1604 Alert Using Same Key-Card Three or More Times in a One Hour Period.md)    排序后滑动窗口判断
* 中等[#1610 Maximum Number of Visible Points](./#1610 Maximum Number of Visible Points.md)    计算坐标角度，使用双指针滑动区间
* 中等[#面试题17.18 Shortest Supersequence LCCI](./#面试题17.18 Shortest Supersequence LCCI.md)    队列左右缩进缩小队列长度