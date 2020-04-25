[toc]

## 队列基本操作

&emsp;队列作为非常常用的数据结构，单独考察队列的题目很少，主要考察循环队列及其队列的深度应用。队列分队首固定的顺序队列、队首不固定的顺序队列及其改进循环队列、链接队列；通常除了循环队列，考题中不需要设计队列，直接使用语言中的集合包。需要理解队列的先进先出思想，结合题目发掘队列的出队、入队规则。循环队列需要注意的是`front`为标志位，需要占用一位，后继是队首，`rear`为队尾标识，指向队尾，特别需要注意队列判空和判满的条件。

* 简单[#3 Longest Substring Without Repeating Characters](./#3 Longest Substring Without Repeating Characters.md)    队列的基本应用
* $\clubs中等$[#209 Minimum Size Subarray Sum](./#209 Minimum Size Subarray Sum.md)    队列双指针思想，可用二分法实现
* 简单[#225 Implement Stack using Queues](./#225 Implement Stack using Queues.md)    队列原理
* $\bigstar$困难[#239 Sliding Window Maximum](./#239 Sliding Window Maximum.md)    单调递减双向队列
* $\clubs$困难[#340 Longest Substring with At Most K Distinct Characters](./#340 Longest Substring with At Most K Distinct Characters.md)    [#3 Longest Substring Without Repeating Characters](./#3 Longest Substring Without Repeating Characters.md)和[#159 Longest Substring with At Most Two Distinct Characters](../散列表/#159 Longest Substring with At Most Two Distinct Characters.md)的进阶
* 简单[#346 Moving Average from Data Stream](./#346 Moving Average from Data Stream.md)    循环队列
* $\clubs$中等[#353 Design Snake Game](./#353 Design Snake Game.md)    队列模拟贪吃蛇的移动进食
* 简单[#362 Design Hit Counter](./#362 Design Hit Counter.md)    队列用于计时
* 中等[#582 Kill Process](./#582 Kill Process.md)    树的广度优先搜索
* 简单[#622 Design Circular Queue](./#622 Design Circular Queue.md)    设计循环队列
* 中等[#641 Design Circular Deque](./#641 Design Circular Deque.md)    设计双向循环队列
* $\bigstar$困难[#862 Shortest Subarray with Sum at Least K](./#862 Shortest Subarray with Sum at Least K.md)    双向队列结合前缀和
* 简单[#933 Number of Recent Calls](./#933 Number of Recent Calls.md)    队列基本操作