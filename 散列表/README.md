[toc]

## 闭散列表

### 线性探测



### 二次探测



## 开散列表

* 简单[#706 Design HashMap](./#706 Design HashMap.md)    开散列表简单设计

## 应用

&emsp;散列表的应用在字符串中主要是保存字符计数，结合队列等求解；在其他应用需要分析映射关系，巧妙应用这种关系。

* $\clubs$困难[#30 Substring with Concatenation of All Words](./#30 Substring with Concatenation of All Words.md)    哈希表记录单词出现次数
* 中等[#36 Valid Sudoku](./#36 Valid Sudoku.md)    数独有效性教校验
* 中等[#49 Group Anagrams](./#49 Group Anagrams.md)    字母异位词分组
* $\clubs$中等[#76 Minimum Window Substring](./#76 Minimum Window Substring.md)    最小覆盖子串
* $\clubs$技巧[#128 Longest Consecutive Sequence](./#128 Longest Consecutive Sequence.md)    集合巧妙记录数字序列
* 简单[#136 Single Number](./#136 Single Number.md)    集合记录消解（非最优解）
* 繁杂[#149 Max Points on a Line](./#149 Max Points on a Line.md)    哈希表记录斜率，注意浮点数运算的精度丢失
* 中等[#159 Longest Substring with At Most Two Distinct Characters](./#159 Longest Substring with At Most Two Distinct Characters.md)    队列与哈希表的简单组合
* 简单[#170 Two Sum III - Data structure design](./#170 Two Sum III - Data structure design.md)    哈希表记录数字及数目
* 简单[#187 Repeated DNA Sequences](./#187 Repeated DNA Sequences.md)    哈希表记录子序列并计数
* 简单[#205 Isomorphic Strings](./#205 Isomorphic Strings.md)    哈希表保存替换关系
* 简单[#217 Contains Duplicate](./#217 Contains Duplicate.md)    哈希表记录数字
* 简单[#219 Contains Duplicate II](./#219 Contains Duplicate II.md)    哈希表记录数字索引
* 简单[#242 Valid Anagram](./#242 Valid Anagram.md)    哈希表保存字符计数
* 中等[#244 Shortest Word Distance II](./#244 Shortest Word Distance II.md)    哈希表保存索引
* 简单[#246 Strobogrammatic Number](./#246 Strobogrammatic Number.md)    中心对称数判断
* 中等[#249 Group Shifted Strings](./#249 Group Shifted Strings.md)    同[#49 Group Anagrams](./#49 Group Anagrams.md)，分析同一组的特征，得到主键
* 简单[#266 Palindrome Permutation](./#266 Palindrome Permutation.md)    哈希表存储字符计数
* $\clubs$中等[#274 H-Index](./#274 H-Index.md)    数组保存论文引用数与计数关系
* 简单[#288 Unique Word Abbreviation](./#288 Unique Word Abbreviation.md)    哈希表的应用
* 简单[#290 Word Pattern](./#290 Word Pattern.md)    哈希表保存映射关系
* 简单[#299 Bulls and Cows](./#299 Bulls and Cows.md)    哈希表保存计数
* 中等[#325 Maximum Size Subarray Sum Equals k](./#325 Maximum Size Subarray Sum Equals k.md)    哈希表记录第一次出现的前缀和
* 简单[#349 Intersection of Two Arrays](./#349 Intersection of Two Arrays.md)    集合去重
* 简单[#350 Intersection of Two Arrays II](./#350 Intersection of Two Arrays II.md)    哈希表记录计数
* 中等[#356 Line Reflection](./#356 Line Reflection.md)    重写散列函数，利用哈希表记录坐标点
* 简单[#359 Logger Rate Limiter](./#359 Logger Rate Limiter.md)    哈希表记录日志和打印时间
* 中等[#380 Insert Delete GetRandom O(1)](./#380 Insert Delete GetRandom O(1).md)    采用哈希表保存数组索引
* $\clubs$困难[#381 Insert Delete GetRandom O(1) - Duplicates allowed](./#381 Insert Delete GetRandom O(1) - Duplicates allowed.md)    哈希表保存索引
* 简单[#383 Ransom Note](./#383 Ransom Note.md)    散列表统计计数
* 简单[#387 First Unique Character in a String](./#387 First Unique Character in a String.md)    哈希表字符计数
* $\clubs$中等[#442 Find All Duplicates in an Array](./#442 Find All Duplicates in an Array.md)    数组下标作为键值
* 中等[#447 Number of Boomerangs](./#447 Number of Boomerangs.md)    哈希表记录遍历值，简化时间复杂度
* 中等[#448 Find All Numbers Disappeared in an Array](./#448 Find All Numbers Disappeared in an Array.md)    [#442 Find All Duplicates in an Array](./#442 Find All Duplicates in an Array.md)的延续
* 中等[#454 4Sum II](./#454 4Sum II.md)    哈希表记录和和计数
* $\clubs$繁杂[#460 LFU Cache](./#460 LFU Cache.md)    最近最少使用策略实现
* $\clubs$技巧[#523 Continuous Subarray Sum](./#523 Continuous Subarray Sum.md)    利用哈希表存储取余后的值
* $\clubs$技巧[#525 Contiguous Array](./#525 Contiguous Array.md)    哈希表存储第一次出现的索引
* $\clubs$困难[#527 Word Abbreviation](./#527 Word Abbreviation.md)    利用字典保存缩写，解决碰撞
* $\bigstar$中等[#548 Split Array with Equal Sum](./#548 Split Array with Equal Sum.md)    集合巧妙化简循环
* 中等[#554 Brick Wall](./#554 Brick Wall.md)    哈希表记录砖缝
* 中等[#560 Subarray Sum Equals K](./#560 Subarray Sum Equals K.md)    使用哈希表记录前缀和
* 简单[#594 Longest Harmonious Subsequence](./#594 Longest Harmonious Subsequence.md)    哈希表统计数字计数
* 中等[#609 Find Duplicate File in System](./#609 Find Duplicate File in System.md)    哈希表记录文件内容及路径
* 简单[#645 Set Mismatch](./#645 Set Mismatch.md)    哈希表记录缺失数字
* 简单[#705 Design HashSet](./#705 Design HashSet.md)    设计集合
* 简单[#771 Jewels and Stones](./#771 Jewels and Stones.md)    哈希表计数字符
* 简单[#819 Most Common Word](./#819 Most Common Word.md)    哈希表单词计数
* 中等[#846 Hand of Straights](./#846 Hand of Straights.md)    哈希表计数判断连续值
* $\bigstar$中等[#1124 Longest Well-Performing Interval](./#1124 Longest Well-Performing Interval.md)    前缀和巧妙的二分查找和哈希表解法
* 中等[#1296 Divide Array in Sets of K Consecutive Numbers](./#1296 Divide Array in Sets of K Consecutive Numbers.md)    同[#846 Hand of Straights](./#846 Hand of Straights.md)
* 简单[#1347 Minimum Number of Steps to Make Two Strings Anagram](./#1347 Minimum Number of Steps to Make Two Strings Anagram.md)    统计字符数目
* 中等[#1429 First Unique Number](./#1429 First Unique Number.md)    哈希表存储前驱节点
* 中等[#1577 Number of Ways Where Square of Number Is Equal to Product of Two Numbers](./#1577 Number of Ways Where Square of Number Is Equal to Product of Two Numbers.md)    哈希表记录乘积（非最优）
* $\clubs$中等[#1583 Count Unhappy Friends](./#1583 Count Unhappy Friends.md)    数组哈希表维护关系亲密度
* $\clubs$中等[#1590 Make Sum Divisible by P](./#1590 Make Sum Divisible by P.md)    哈希表保存数组取余值
* 中等[#1657 Determine if Two Strings Are Close](./#1657 Determine if Two Strings Are Close.md)    数组哈希表保存计数值
* 中等[#1726 Tuple with Same Product](./#1726 Tuple with Same Product.md)    哈希表记录乘积
* 中等[#面试题17.05 Find Longest Subarray LCCI](./#面试题17.05 Find Longest Subarray LCCI.md)    哈希表记录前缀差值
* 困难[#面试题17.26 Sparse Similarity LCCI](./#面试题17.26 Sparse Similarity LCCI.md)    哈希表记录单词与文档关系