[toc]

## 枚举法



## 贪婪法

* $\clubs$困难[#45 Jump Game II](./#45 Jump Game II.md)    [#55 Jump Game](./#55 Jump Game.md)的进阶，巧妙的贪心思路及规律
* 中等[#55 Jump Game](./#55 Jump Game.md)    发现规律从尾遍历
* 中等[#57 Insert Interval](./#57 Insert Interval.md)    从头遍历判断区间
* 简单[#122 Best Time to Buy and Sell Stock II](./#122 Best Time to Buy and Sell Stock II.md)    全局解由局部解组成，最基本的贪婪特征
* $\clubs$中等[#134 Gas Station](./#134 Gas Station.md)    整体特征的分解总结
* $\bigstar$技巧[#135 Candy](./#135 Candy.md)    峰顶值的处理
* $\bigstar$困难[#321 Create Maximum Number](./#321 Create Maximum Number.md)    利用贪婪策略查询两个数组的最大组合
* $\bigstar$困难[#659 Split Array into Consecutive Subsequences](./#659 Split Array into Consecutive Subsequences.md)    贪婪拼接连续数字

## 分治法

* $\bigstar$简单[#53 Maximum Subarray](./#53 Maximum Subarray.md)    最大连续子序列问题
* $\bigstar$困难[#218 The Skyline Problem](./#218 The Skyline Problem.md)    归并排序的分治思路应用
* $\clubs$中等[#241 Different Ways to Add Parentheses](./#241 Different Ways to Add Parentheses.md)    表达式根据操作符分治

## 动态规划

* 简单[#62 Unique Paths](./#62 Unique Paths.md)    基本动态规划问题
* 中等[#63 Unique Paths II](./#63 Unique Paths II.md)    [#62 Unique Paths](./#62 Unique Paths.md)的变形
* 简单[#64 Minimum Path Sum](./#64 Minimum Path Sum.md)    基本动态规划问题
* 简单[#70 Climbing Stairs](./#70 Climbing Stairs.md)    经典动态规划问题
* $\clubs$困难[#72 Edit Distance](./#72 Edit Distance.md)    经典动态规划的字符串应用
* 中等[#91 Decode Ways](./#91 Decode Ways.md)    边角条件繁杂的动态规划应用
* 中等[#120 Triangle](./#120 Triangle.md)    非矩形的动态规划
* $\clubs$中等[#139 Word Break](./#139 Word Break.md)    字符串匹配的动态规划
* $\bigstar$困难[#174 Dungeon Game](./#174 Dungeon Game.md)    [#64 Minimum Path Sum](./#64 Minimum Path Sum.md)的延伸，遍历方向很重要
* $\clubs$简单[#198 House Robber](./#198 House Robber.md)    巧妙的动态规划简化
* $\bigstar$困难[#312 Burst Balloons](./#312 Burst Balloons.md)    逆向思维动态规划
* $\bigstar$中等[#376 Wiggle Subsequence](./#376 Wiggle Subsequence.md)    巧妙地规律总结
* $\clubs$困难[#471 Encode String with Shortest Length](./#471 Encode String with Shortest Length.md)    字符串最短编码
* 中等[#562 Longest Line of Consecutive One in Matrix](./#562 Longest Line of Consecutive One in Matrix.md)    记录之前位置的连续序列信息
* $\bigstar$困难[#903 Valid Permutations for DI Sequence](./#903 Valid Permutations for DI Sequence.md)    繁杂的动态规划抽象
* $\clubs$困难[#1235 Maximum Profit in Job Scheduling](./#1235 Maximum Profit in Job Scheduling.md)    按结束时间动态规划

## 回溯法

* $\bigstar$困难[#10 Regular Expression Matching](./#10 Regular Expression Matching.md)    可动态规划优化的回溯解法
* 中等[#17 Letter Combinations of a Phone Number](./#17 Letter Combinations of a Phone Number.md)    枚举回溯所有组合
* $\clubs$中等[#22 Generate Parentheses](./#22 Generate Parentheses.md)    根据括号组合特性进行回溯
* 中等[#39 Combination Sum](./#39 Combination Sum.md)    通过排序去除重复组合，减少试探次数
* 中等[#40 Combination Sum II](./#40 Combination Sum II.md)    [#39 Combination Sum](./#39 Combination Sum.md)进阶，每个元素只能选一次
* $\bigstar$困难[#44 Wildcard Matching](./#44 Wildcard Matching.md)    经典的模式匹配问题，双字符串遍历回溯
* 中等[#46 Permutations](./#46 Permutations.md)    排列组合的回溯枚举
* $\clubs$中等[#47 Permutations II](./#47 Permutations II.md)    [#46 Permutations](./#46 Permutations.md)的进阶，存在重复值的组合
* $\bigstar$中等[#51 N-Queens](./#51 N-Queens.md)    经典回溯问题
* 中等[#52 N-Queens II](./#52 N-Queens II.md)    $n$皇后的简化
* 中等[#93 Restore IP Addresses](./#93 Restore IP Addresses.md)    条件判断减少回溯次数
* $\clubs$繁杂[#282 Expression Add Operators](./#282 Expression Add Operators.md)    回溯插入操作符使得结果与目标值一致
* $\clubs$中等[#320 Generalized Abbreviation](./#320 Generalized Abbreviation.md)    字符串回溯
* $\clubs$中等[#351 Android Unlock Patterns](./#351 Android Unlock Patterns.md)    矩形回溯遍历的变形
* 繁杂[#411 Minimum Unique Word Abbreviation](./#411 Minimum Unique Word Abbreviation.md)    [#320 Generalized Abbreviation](./#320 Generalized Abbreviation.md)  和[#408 Valid Word Abbreviation](../字符串/#408 Valid Word Abbreviation.md)的结合
* $\bigstar$困难[#546 Remove Boxes](./#546 Remove Boxes.md)    存储计算结果的回溯

## 随机算法

