[toc]

## 枚举法

&emsp;枚举法适合于解的候选者是有限的、可枚举的场合。枚举法就是对可能是解的众多候选者按某种顺序进行逐一枚举和检验，从中找出符合要求的候选解作为问题的解。

## 贪婪法

&emsp;&emsp;贪婪法是一种不追求最优解，只希望得到较为满意的解的方法。因为它省去了为找最优解而穷尽所有可能所需的时间，因而贪婪法一般可以快速得到满意的解。贪婪法在求解过程的每一步都选取一个局部最优的策略，把问题规模缩小，最后把每一步的结果合并起来形成一个全局解。贪婪法的基本步骤为：

* 从某个初始解出发；
* 采用迭代的过程，当可以向目标前进一步时，就根据局部最优策略，得到一个部分解，问题规模就缩小了；
* 将所有解综合起来。



* $\clubs$困难[#45 Jump Game II](./#45 Jump Game II.md)    [#55 Jump Game](./#55 Jump Game.md)的进阶，巧妙的贪心思路及规律
* 中等[#55 Jump Game](./#55 Jump Game.md)    发现规律从尾遍历
* 中等[#57 Insert Interval](./#57 Insert Interval.md)    从头遍历判断区间
* 简单[#122 Best Time to Buy and Sell Stock II](./#122 Best Time to Buy and Sell Stock II.md)    全局解由局部解组成，最基本的贪婪特征
* $\clubs$中等[#134 Gas Station](./#134 Gas Station.md)    整体特征的分解总结
* $\bigstar$技巧[#135 Candy](./#135 Candy.md)    峰顶值的处理
* $\bigstar$困难[#321 Create Maximum Number](./#321 Create Maximum Number.md)    利用贪婪策略查询两个数组的最大组合
* $\clubs$中等[#406 Queue Reconstruction by Height](./#406 Queue Reconstruction by Height.md)    身高降序计数增序
* 中等[#452 Minimum Number of Arrows to Burst Balloons](./#452 Minimum Number of Arrows to Burst Balloons)    贪婪发统计区间交集数目
* $\bigstar$困难[#659 Split Array into Consecutive Subsequences](./#659 Split Array into Consecutive Subsequences.md)    贪婪拼接连续数字
* $\clubs$中等[#910 Smallest Range II](./#910 Smallest Range II.md)    巧妙地数学规律发现
* $\bigstar$困难[#630 Course Schedule III](./#630 Course Schedule III.md)    课程时长的贪婪思路
* $\clubs$中等[#870 Advantage Shuffle](./#870 Advantage Shuffle.md)    排序遍历，贪婪搭配
* 中等[#955 Delete Columns to Make Sorted II](./#955 Delete Columns to Make Sorted II.md)    字符串移除字符使之变为字典序
* $\clubs$技巧[#1247 Minimum Swaps to Make Strings Equal](./#1247 Minimum Swaps to Make Strings Equal.md)    不匹配对交换情况贪婪选择
* $\clubs$困难[#1326 Minimum Number of Taps to Open to Water a Garden](./#1326 Minimum Number of Taps to Open to Water a Garden.md)    区间嵌套可达的抽象
* $\clubs$技巧[#1520 Maximum Number of Non-Overlapping Substrings](./#1520 Maximum Number of Non-Overlapping Substrings.md)    根据起始字符统计子字符串区间并贪婪搜索最大不相交区间数目
* $\clubs$中等[#1536 Minimum Swaps to Arrange a Binary Grid](./#1536 Minimum Swaps to Arrange a Binary Grid.md)    最小交换行的次数使得矩阵为上三角

## 分治法

&emsp;分治法基本思想是将一个大问题分成若干个小问题，然后由小问题的解构造出大问题的解。把大问题分成小问题称为“分”，从小问题的解构造大问题的解称为“治”。分治法通常都是用递归实现的。在问题分解过程中，往往将一个大问题分成若干个同类的小问题。

> 需注意原问题分解成的子问题可以独立求解，子问题之间没有相关性，这一点是分治算法跟动态规划的明显区别；其次可以将子问题合并成原问题，而这个合并操作的复杂度不能太高，否则就起不到减小算法总体复杂度的效果。

* $\bigstar$简单[#53 Maximum Subarray](./#53 Maximum Subarray.md)    最大连续子序列问题
* $\bigstar$困难[#218 The Skyline Problem](./#218 The Skyline Problem.md)    归并排序的分治思路应用
* $\clubs$中等[#241 Different Ways to Add Parentheses](./#241 Different Ways to Add Parentheses.md)    表达式根据操作符分治
* 中等[#395 Longest Substring with At Least K Repeating Characters](./#395 Longest Substring with At Least K Repeating Characters.md)    分治遍历判断子串
* 中等[#1274 Number of Ships in a Rectangle](./#1274 Number of Ships in a Rectangle.md)    二等分矩形区间分别判断

## 动态规划

&emsp;在实际应用中经常会遇到一个复杂的问题不能简单地分成几个子问题，而是会分解出一系列同类的子问题。如果用分治法就会使得递归调用的次数早指数增长。为了节约重复求相同子问题的时间，可引入一个数组，从小到大计算每个子问题的解。不管每个子问题的解对最终解是否有用，都把它保存于该数组中。当解决一些大问题时，将此大问题分解成一些同类的小问题。而此时，不需要用递归调用去获得小问题的解，而可以直接从数组中取出小问题的解。这就是动态规划的基本思想。

> 能用动态规划解决的问题，需要满足三个特征，最优子结构、无后效性和重复子问题。一般能用动态规划解决的问题，都可以使用回溯算法的暴力搜索解决，可先使用回溯法总结规律，再用动态规划实现。

* 简单[#62 Unique Paths](./#62 Unique Paths.md)    基本动态规划问题
* 中等[#63 Unique Paths II](./#63 Unique Paths II.md)    [#62 Unique Paths](./#62 Unique Paths.md)的变形
* 简单[#64 Minimum Path Sum](./#64 Minimum Path Sum.md)    基本动态规划问题
* 简单[#70 Climbing Stairs](./#70 Climbing Stairs.md)    经典动态规划问题
* $\clubs$困难[#72 Edit Distance](./#72 Edit Distance.md)    经典动态规划的字符串应用
* $\clubs$困难[#87 Scramble String](./#87 Scramble String.md)    从回溯到动态规划
* 中等[#91 Decode Ways](./#91 Decode Ways.md)    边角条件繁杂的动态规划应用
* $\clubs$困难[#97 Interleaving String](./#97 Interleaving String.md)    交错字符串组成判断
* 困难[#115 Distinct Subsequences](./#115 Distinct Subsequences.md)    子序列匹配数目
* 简单[#119 Pascal's Triangle II](./#119 Pascal's Triangle II.md)    经典动态规划问题
* $\bigstar$困难[#123 Best Time to Buy and Sell Stock III](./#123 Best Time to Buy and Sell Stock III.md)    巧妙的状态转化
* 中等[#120 Triangle](./#120 Triangle.md)    非矩形的动态规划
* $\clubs$中等[#139 Word Break](./#139 Word Break.md)    字符串匹配的动态规划
* 困难[#140 Word Break II](./#140 Word Break II.md)    [#139 Word Break](./#139 Word Break.md)的进阶
* $\bigstar$困难[#174 Dungeon Game](./#174 Dungeon Game.md)    [#64 Minimum Path Sum](./#64 Minimum Path Sum.md)的延伸，遍历方向很重要
* 困难[#188 Best Time to Buy and Sell Stock IV](./#188 Best Time to Buy and Sell Stock IV.md)    [#122 Best Time to Buy and Sell Stock II.md](./#122 Best Time to Buy and Sell Stock II.md)和[#123 Best Time to Buy and Sell Stock III](./#123 Best Time to Buy and Sell Stock III.md)的结合
* 简单[#198 House Robber](./#198 House Robber.md)    巧妙的动态规划简化
* $\clubs$中等[#213 House Robber II](./#213 House Robber II.md)    [#198 House Robber](./#198 House Robber.md)思路的延续
* 中等[#221 Maximal Square](./#221 Maximal Square.md)    最大正方形
* 简单[#256 Paint House](./#256 Paint House.md)    房子染色最小代价
* $\clubs$困难[#265 Paint House II](./#265 Paint House II.md)    [#256 Paint House](./#256 Paint House.md)普遍情况
* 简单[#276 Paint Fence](./#276 Paint Fence.md)    着色方案数目
* 中等[#279 Perfect Squares](./#279 Perfect Squares.md)    动态规划（非最优）
* 中等[#309 Best Time to Buy and Sell Stock with Cooldown](./#309 Best Time to Buy and Sell Stock with Cooldown.md)    [#123 Best Time to Buy and Sell Stock III](./#123 Best Time to Buy and Sell Stock III.md)无限制交易且存在冷冻期
* $\bigstar$困难[#312 Burst Balloons](./#312 Burst Balloons.md)    逆向思维动态规划
* 中等[#343 Integer Break](./#343 Integer Break.md)    动态规划（非最优）
* $\bigstar$中等[#376 Wiggle Subsequence](./#376 Wiggle Subsequence.md)    巧妙地规律总结
* $\clubs$困难[#403 Frog Jump](./#403 Frog Jump.md)    非数组动态规划
* 中等[#413 Arithmetic Slices](./#413 Arithmetic Slices.md)    等差序列子数组计数
* $\clubs$困难[#471 Encode String with Shortest Length](./#471 Encode String with Shortest Length.md)    字符串最短编码
* 中等[#494 Target Sum](./#494 Target Sum.md)    从回溯到动态规划
* 中等[#516 Longest Palindromic Subsequence](./#516 Longest Palindromic Subsequence.md)    回文子序列
* $\bigstar$困难[#552 Student Attendance Record II](./#552 Student Attendance Record II.md)    发现递归规律使用动态规划实现（最优算法为状态机）
* 中等[#562 Longest Line of Consecutive One in Matrix](./#562 Longest Line of Consecutive One in Matrix.md)    记录之前位置的连续序列信息
* $\clubs$困难[#629 K Inverse Pairs Array](./#629 K Inverse Pairs Array.md)    转移方程的归纳总结
* 中等[#651 4 Keys Keyboard](./#651 4 Keys Keyboard.md)    数学方式优化
* 困难[#568 Maximum Vacation Days](./#568 Maximum Vacation Days.md)    动态规划复杂应用
* 中等[#688 Knight Probability in Chessboard](./#688 Knight Probability in Chessboard.md)    国际象棋马在棋盘内的概率
* 中等[#714 Best Time to Buy and Sell Stock with Transaction Fee](./#714 Best Time to Buy and Sell Stock with Transaction Fee.md)    [#123 Best Time to Buy and Sell Stock III](./#123 Best Time to Buy and Sell Stock III.md)扣除手续费
* $\clubs$中等[#718 Maximum Length of Repeated Subarray](./#718 Maximum Length of Repeated Subarray.md)    动态规划求最大子数组长度（非最优）
* $\clubs$困难[#730 Count Different Palindromic Subsequences](./#730 Count Different Palindromic Subsequences.md)    不重复的回文子序列
* 中等[#764 Largest Plus Sign](./#764 Largest Plus Sign.md)    矩形遍历有关的动态规划
* $\clubs$技巧[#788 Rotated Digits](./#788 Rotated Digits.md)    巧妙的动态规划思路
* $\clubs$中等[#818 Race Car](./#818 Race Car.md)    赛车最短指令
* $\clubs$中等[#837 New 21 Game](./#837 New 21 Game.md)    概率逆向更新的动态规划
* $\bigstar$中等[#873 Length of Longest Fibonacci Subsequence](./#873 Length of Longest Fibonacci Subsequence.md)    最长斐波那契子序列
* $\bigstar$困难[#887 Super Egg Drop](./#887 Super Egg Drop.md)    动态规划经典问题双蛋问题
* $\bigstar$困难[#903 Valid Permutations for DI Sequence](./#903 Valid Permutations for DI Sequence.md)    繁杂的动态规划抽象
* 中等[#935 Knight Dialer](./#935 Knight Dialer.md)    有限状态机
* $\clubs$困难[#943 Find the Shortest Superstring](./#943 Find the Shortest Superstring.md)    路径压缩
* $\clubs$困难[#956 Tallest Billboard](./#956 Tallest Billboard.md)    动态规划数组表示支架差距
* $\bigstar$困难[#1000 Minimum Cost to Merge Stones](./#1000 Minimum Cost to Merge Stones.md)    区间动态规划
* 中等[#1035 Uncrossed Lines](./#1035 Uncrossed Lines.md)    最长公共子序列的应用
* $\clubs$中等[#1039 Minimum Score Triangulation of Polygon](./#1039 Minimum Score Triangulation of Polygon.md)    几何规律
* 困难[#1092 Shortest Common Supersequence](./#1092 Shortest Common Supersequence.md)    最长公共子序列的应用
* 困难[#1125 Smallest Sufficient Team](./#1125 Smallest Sufficient Team)    路径动态规划与倒推
* 中等[#1139 Largest 1-Bordered Square](./#1139 Largest 1-Bordered Square.md)    存储记录连续的左侧和上方结点数目
* 中等[#1143 Longest Common Subsequence](./#1143 Longest Common Subsequence.md)    最长公共子序列
* 中等[#1155 Number of Dice Rolls With Target Sum](./#1155 Number of Dice Rolls With Target Sum.md)    动态规划记录目标和数目
* $\clubs$中等[#1186 Maximum Subarray Sum with One Deletion](./#1186 Maximum Subarray Sum with One Deletion.md)    动态规划记录删除一个数的最大值
* $\clubs$困难[#1187 Make Array Strictly Increasing](./#1187 Make Array Strictly Increasing.md)    动态规划记录递增序列末尾值
* 中等[#1223 Dice Roll Simulation](./#1223 Dice Roll Simulation.md)    计数序列末尾值
* 中等[#1230 Toss Strange Coins](./#1230 Toss Strange Coins.md)    概率的动态规划问题
* $\clubs$困难[#1235 Maximum Profit in Job Scheduling](./#1235 Maximum Profit in Job Scheduling.md)    按结束时间动态规划
* 困难[#1246 Palindrome Removal](./#1246 Palindrome Removal.md)    回文字符串的动态规划规律
* $\clubs$困难[#1259 Handshakes That Don't Cross](./#1259 Handshakes That Don't Cross.md)    巧妙的边角条件处理
* 中等[#1278 Palindrome Partitioning III](./#1278 Palindrome Partitioning III.md)    改变字符使之变为回文的最少次数
* 困难[#1312 Minimum Insertion Steps to Make a String Palindrome](./#1312 Minimum Insertion Steps to Make a String Palindrome.md)    区间动态规划
* 困难[#1388 Pizza With 3n Slices](./#1388 Pizza With 3n Slices.md)    [#213 House Robber II](./#213 House Robber II.md)的变形
* 困难[#1411 Number of Ways to Paint N × 3 Grid](./#1411 Number of Ways to Paint N × 3 Grid.md)    抽象每行填充状态
* 中等[#1458 Max Dot Product of Two Subsequences](./#1458 Max Dot Product of Two Subsequences.md)    最大子序列点积和
* $\clubs$困难[#1478 Allocate Mailboxes](./#1478 Allocate Mailboxes.md)    分配$k$个邮箱最短距离和
* $\bigstar$困难[#1494 Parallel Courses II](./#1494 Parallel Courses II.md)    状态压缩与位运算结合的动态规划
* $\bigstar$困难[#1531 String Compression II](./#1531 String Compression II.md)    最多删除$k$个字符后的最短压缩编码
* 中等[#1547 Minimum Cost to Cut a Stick](./#1547 Minimum Cost to Cut a Stick.md)    区间动态规划
* $\bigstar$困难[#1563 Stone Game V](./#1563 Stone Game V.md)    动态规划的深层优化

## 回溯法

&emsp;回溯法也称试探法，该方法首先暂时放弃问题规模大小的限制，从最小规模开始将问题的候选解按某种顺序逐一枚举和检验。当发现候选解不可能是解时，就选择下一候选解。如果当前候选解除了不满足规模要求外，满足其他所有要求时，则继续扩大当前候选解的规模，并继续试探。如果当前的候选解满足包括问题规模在内的所有要求时，该候选解就是问题的一个解。寻找下一候选解的过程称为回溯。扩大当前候选解的规模，并继续试探的过程称为向前试探。

* $\bigstar$困难[#10 Regular Expression Matching](./#10 Regular Expression Matching.md)    可动态规划优化的回溯解法
* 中等[#17 Letter Combinations of a Phone Number](./#17 Letter Combinations of a Phone Number.md)    枚举回溯所有组合
* $\clubs$中等[#22 Generate Parentheses](./#22 Generate Parentheses.md)    根据括号组合特性进行回溯
* $\bigstar$困难[#37 Sudoku Solver](./#37 Sudoku Solver.md)    解数独，经典回溯问题
* 中等[#39 Combination Sum](./#39 Combination Sum.md)    通过排序去除重复组合，减少试探次数
* 中等[#40 Combination Sum II](./#40 Combination Sum II.md)    [#39 Combination Sum](./#39 Combination Sum.md)进阶，每个元素只能选一次
* $\bigstar$困难[#44 Wildcard Matching](./#44 Wildcard Matching.md)    经典的模式匹配问题，双字符串遍历回溯
* 中等[#46 Permutations](./#46 Permutations.md)    排列组合的回溯枚举
* $\clubs$中等[#47 Permutations II](./#47 Permutations II.md)    [#46 Permutations](./#46 Permutations.md)的进阶，存在重复值的组合
* $\bigstar$中等[#51 N-Queens](./#51 N-Queens.md)    经典回溯问题
* 中等[#52 N-Queens II](./#52 N-Queens II.md)    $n$皇后的简化
* $\clubs$中等[#60 Permutation Sequence](./#60 Permutation Sequence.md)    寻找第$k$个组合
* 中等[#77 Combinations](./#77 Combinations.md)    组合的回溯
* $\clubs$中等[#78 Subsets](./#78 Subsets.md)    回溯查找所有子集和
* 中等[#79 Word Search](./#79 Word Search.md)    字符串回溯比较
* 中等[#89 Gray Code](./#89 Gray Code.md)    回溯尝试
* 中等[#90 Subsets II](./#90 Subsets II.md)    排列排序消除重复
* 中等[#93 Restore IP Addresses](./#93 Restore IP Addresses.md)    条件判断减少回溯次数
* $\clubs$中等[#131 Palindrome Partitioning](./#131 Palindrome Partitioning.md)    回溯尝试所有回文子串
* 中等[#216 Combination Sum III](./#216 Combination Sum III.md)    回溯数字组合
* 中等[#247 Strobogrammatic Number II](./#247 Strobogrammatic Number II.md)    中心对称数字回溯生成
* 繁杂[#248 Strobogrammatic Number III](./#248 Strobogrammatic Number III.md)    [#247 Strobogrammatic Number II](./#247 Strobogrammatic Number II.md)进阶统计区间内的对称数
* 中等[#267 Palindrome Permutation II](./#267 Palindrome Permutation II.md)    回溯生成字符串
* $\clubs$繁杂[#282 Expression Add Operators](./#282 Expression Add Operators.md)    回溯插入操作符使得结果与目标值一致
* $\clubs$困难[#301 Remove Invalid Parentheses](./#301 Remove Invalid Parentheses.md)    统计删除括号数，回溯删除
* $\clubs$中等[#320 Generalized Abbreviation](./#320 Generalized Abbreviation.md)    字符串回溯
* $\clubs$中等[#351 Android Unlock Patterns](./#351 Android Unlock Patterns.md)    矩形回溯遍历的变形
* $\clubs$中等[#386 Lexicographical Numbers](./#386 Lexicographical Numbers.md)    经典回溯题目
* 繁杂[#411 Minimum Unique Word Abbreviation](./#411 Minimum Unique Word Abbreviation.md)    [#320 Generalized Abbreviation](./#320 Generalized Abbreviation.md)  和[#408 Valid Word Abbreviation](../字符串/#408 Valid Word Abbreviation.md)的结合
* 中等[#464 Can I Win](./#464 Can I Win.md)    状态压缩记忆化回溯
* 困难[#465 Optimal Account Balancing](./#465 Optimal Account Balancing.md)    NP难问题，回溯暴力尝试
* $\clubs$困难[#488 Zuma Game](./#488 Zuma Game.md)    NP难问题，暴力回溯
* $\bigstar$困难[#546 Remove Boxes](./#546 Remove Boxes.md)    存储计算结果的回溯
* 中等[#638 Shopping Offers](./#638 Shopping Offers.md)    背包问题回溯
* $\clubs$困难[#679 24 Game](./#679 24 Game.md)    表达式的配列组合特征
* $\clubs$中等[#698 Partition to K Equal Sum Subsets](./#698 Partition to K Equal Sum Subsets.md)    巧妙剪枝提升时间性能
* 简单[#784 Letter Case Permutation](./#784 Letter Case Permutation.md)    回溯尝试大小写
* $\clubs$困难[#805 Split Array With Same Average](./#805 Split Array With Same Average.md)    剪枝回溯
* 困难[#980 Unique Paths III](./#980 Unique Paths III.md)    记忆化回溯，路径二进制压缩
* $\clubs$中等[#1079 Letter Tile Possibilities](./#1079 Letter Tile Possibilities.md)    回溯中去重技巧
* 中等[#1087 Brace Expansion](./#1087 Brace Expansion.md)    回溯尝试所有组合
* 繁杂[#1088 Confusing Number II](./#1088 Confusing Number II.md)    需注意边界及数值溢出的处理
* $\clubs$中等[#1239 Maximum Length of a Concatenated String with Unique Characters](./#1239 Maximum Length of a Concatenated String with Unique Characters.md)    字符串顺序无关的回溯
* $\clubs$困难[#1240 Tiling a Rectangle with the Fewest Squares](./#1240 Tiling a Rectangle with the Fewest Squares.md)    暴力回溯填充矩形
* 中等[#1255 Maximum Score Words Formed by Letters](./#1255 Maximum Score Words Formed by Letters.md)    回溯尝试单词组合
* $\clubs$繁杂[#1307 Verbal Arithmetic Puzzle](./#1307 Verbal Arithmetic Puzzle.md)    剪枝优化暴力回溯
* 困难[#1340 Jump Game V](./#1340 Jump Game V.md)    记忆化回溯
* 中等[#1553 Minimum Number of Days to Eat N Oranges](./#1553 Minimum Number of Days to Eat N Oranges.md)    回溯的剪枝优化
* 中等[#面试题08.02 Robot in a Grid LCCI](./#面试题08.02 Robot in a Grid LCCI.md)    回溯剪枝优化

## 随机算法

&emsp;随机算法，是指在算法中至少有一个地方使用随机数作决策。随机算法的时间复杂度一般用期望的运行时间来表示。一般假设随机数的选择是均匀分布的。

* 中等[#382 Linked List Random Node](./#382 Linked List Random Node.md)    蓄水池抽样算法
* $\clubs$中等[#384 Shuffle an Array](./#384 Shuffle an Array.md)    洗牌算法
* $\clubs$中等[#398 Random Pick Index](./#398 Random Pick Index.md)    蓄水池抽样算法
* $\clubs$中等[#470 Implement Rand10() Using Rand7()](./#470 Implement Rand10() Using Rand7().md)    拒绝采样
* $\clubs$中等[#478 Generate Random Point in a Circle](./#478 Generate Random Point in a Circle.md)    拒绝采样
* $\bigstar$困难[#1206 Design Skiplist](./#1206 Design Skiplist.md)    设计跳表