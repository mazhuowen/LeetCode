[toc]

## 设计

* 简单[#157 Read N Characters Given Read4](./#157 Read N Characters Given Read4.md)    循环查询
* 困难[#158 Read N Characters Given Read4 II - Call multiple times](./#158 Read N Characters Given Read4 II - Call multiple times.md)    寄存查询剩余的结果
* 中等[#251 Flatten 2D Vector](./#251 Flatten 2D Vector.md)    设计二维数组迭代器
* 简单[#281 Zigzag Iterator](./#281 Zigzag Iterator.md)    锯齿迭代器
* 简单[#284 Peeking Iterator](./#284 Peeking Iterator.md)    设计支持预取的迭代器
* 简单[#348 Design Tic-Tac-Toe](./#348 Design Tic-Tac-Toe.md)    矩形行、列、对角线的表示
* 中等[#535 Encode and Decode TinyURL](./#535 Encode and Decode TinyURL.md)    短链系统设计
* 简单[#604 Design Compressed String Iterator](./#604 Design Compressed String Iterator.md)    设计压缩字符串迭代器
* $\clubs$困难[#843 Guess the Word](./#843 Guess the Word.md)    启发式极小极大算法
* $\clubs$困难[#855 Exam Room](./#855 Exam Room.md)    红黑数动态保存区间长度
* $\bigstar$困难[#895 Maximum Frequency Stack](./#895 Maximum Frequency Stack.md)    字典加栈的组合巧妙设计频率栈
* 中等[#1146 Snapshot Array](./#1146 Snapshot Array.md)    设计支持快照的类数组数据结构
* 中等[#1166 Design File System](./#1166 Design File System.md)    设计文件系统
* 中等[#1396 Design Underground System](./#1396 Design Underground System.md)    设计地铁统计系统
* 中等[#1500 Design a File Sharing System](./#1500 Design a File Sharing System.md)    设计文件共享系统
* 简单[#1603 Design Parking System](./#1603 Design Parking System.md)    计数停车

## 逻辑

* $\clubs$中等[#277 Find the Celebrity](./#277 Find the Celebrity.md)    查找名人
* 中等[#481 Magical String](./#481 Magical String.md)    模拟神奇字符串
* $\bigstar$困难[#517 Super Washing Machines](./#517 Super Washing Machines.md)    最少操作次数
* 简单[#598 Range Addition II](./#598 Range Addition II.md)    多次操作的重叠区域
* $\clubs$中等[#681 Next Closest Time](./#681 Next Closest Time.md)    最近的时间
* $\bigstar$困难[#782 Transform to Chessboard](./#782 Transform to Chessboard.md)    行列交换规律
* 中等[#794 Valid Tic-Tac-Toe State](./#794 Valid Tic-Tac-Toe State.md)    井字棋是否有效验证
* $\clubs$困难[#927 Three Equal Parts](./#927 Three Equal Parts.md)    三等份数组
* 困难[#1121 Divide Array Into Increasing Sequences](./#1121 Divide Array Into Increasing Sequences.md)    相同数字最大数目
* 中等[#1538 Guess the Majority in a Hidden Array](./#1538 Guess the Majority in a Hidden Array.md)    查找基准类并检测
* 简单[#1560 Most Visited Sector in  a Circular Track](./#1560 Most Visited Sector in  a Circular Track.md)    经过最多的扇区
* $\clubs$中等[#1562 Find Latest Group of Size M](./#1562 Find Latest Group of Size M.md)    记录合并过程中的线段端点和长度
* 中等[#1599 Maximum Profit of Operating a Centennial Wheel](./#1599 Maximum Profit of Operating a Centennial Wheel.md)    按照题意模拟

## 技巧

* $\clubs$中等[#54 Spiral Matrix](./#54 Spiral Matrix.md)    螺旋矩阵
* 中等[#59 Spiral Matrix II](./#59 Spiral Matrix II.md)    [#54 Spiral Matrix](./#54 Spiral Matrix.md)的变形
* $\clubs$中等[#73 Set Matrix Zeroes](./#73 Set Matrix Zeroes.md)    利用矩形空间做标识
* $\bigstar$中等[#152 Maximum Product Subarray](./#152 Maximum Product Subarray.md)    正向反向遍历计算最大值
* $\clubs$中等[#238 Product of Array Except Self](./#238 Product of Array Except Self.md)    前缀积
* 中等[#289 Game of Life](./#289 Game of Life.md)   新增状态巧妙利用空间
* $\clubs$中等[#304 Range Sum Query 2D - Immutable](./#304 Range Sum Query 2D - Immutable.md)    前缀和思想在二维数组的应用
* 简单[#724 Find Pivot Index](./#724 Find Pivot Index.md)    前缀和
* 中等[#769 Max Chunks To Make Sorted](./#769 Max Chunks To Make Sorted.md)    连续数组前缀和规律
* $\clubs$困难[#828 Count Unique Characters of All Substrings of a Given String](./#828 Count Unique Characters of All Substrings of a Given String.md)    计数每个字符不重复区间组合
* $\clubs$中等[#885 Spiral Matrix III](./#885 Spiral Matrix III.md)    [#54 Spiral Matrix](./#54 Spiral Matrix.md)的进阶，从内往外旋转
* 中等[#939 Minimum Area Rectangle](./#939 Minimum Area Rectangle.md)    拟对角线遍历验证
* 中等[#1292 Maximum Side Length of a Square with Sum Less than or Equal to Threshold](./#1292 Maximum Side Length of a Square with Sum Less than or Equal to Threshold.md)    矩形前缀和优化
* 中等[#1352 Product of the Last K Numbers](./#1352 Product of the Last K Numbers.md)    前缀积变型
* 简单[#1375 Bulb Switcher III](./#1375 Bulb Switcher III.md)    编号求和比较
* $\clubs$困难[#面试题17.24 最大子矩阵](./#面试题17.24 最大子矩阵.md)    列前缀和状态压缩

## 多线程

* 简单[#1114 Print in Order](./#1114 Print in Order.md)    线程间变量共享通信
* 中等[#1115 Print FooBar Alternately](./#1115 Print FooBar Alternately.md)    `ReentrantLock`线程互相唤醒
* $\clubs$中等[#1116 Print Zero Even Odd](./#1116 Print Zero Even Odd.md)    信号量的应用
* 中等[#1117 Building H2O](./#1117 Building H2O.md)    信号量协调通信
* $\clubs$中等[#1188 Design Bounded Blocking Queue](./#1188 Design Bounded Blocking Queue.md)    入队出队间通信
* 中等[#1195 Fizz Buzz Multithreaded](./#1195 Fizz Buzz Multithreaded.md)    多线程同步通信问题
* $\bigstar$中等[#1226 The Dining Philosophers](./#1226 The Dining Philosophers.md)    哲学家就餐问题
* $\clubs$中等[#1242 Web Crawler Multithreaded](./#1242 Web Crawler Multithreaded.md)    独占锁细化锁粒度
* 简单[#1279 Traffic Light Controlled Intersection](./#1279 Traffic Light Controlled Intersection.md)    独占锁

## 数据库

* 简单[#175 Combine Two Tables](./#175 Combine Two Tables.md)    LEFT JOIN语句
* 简单[#176 Second Highest Salary](./#176 Second Highest Salary.md)    LIMIT和OFFSET的组合
* $\clubs$中等[#177 Nth Highest Salary](./#177 Nth Highest Salary.md)    LIMIT的使用
* $\bigstar$中等[#178 Rank Scores](./#178 Rank Scores.md)    DENSE_RANK函数应用
* 中等[#180 Consecutive Numbers](./#180 Consecutive Numbers.md)    自连接查询
* 简单[#181 Employees Earning More Than Their Managers](./#181 Employees Earning More Than Their Managers.md)    自连接查询
* 简单[#182 Duplicate Emails](./#182 Duplicate Emails.md)    GROUP BY和HAVING的组合
* 简单[#183 Customers Who Never Order](./#183 Customers Who Never Order.md)    NOT IN使用
* 中等[#184 Department Highest Salary](./#184 Department Highest Salary.md)    IN技巧
* $\clubs$困难[#185 Department Top Three Salaries](./#185 Department Top Three Salaries.md)    自连接
* 简单[#196 Delete Duplicate Emails](./#196 Delete Duplicate Emails.md)    自连接
* 简单[#197 Rising Temperature](./#197 Rising Temperature.md)    DATEDIFF函数应用
* 中等[#262 Trips and Users](./#262 Trips and Users.md)    自连接统计计数
* 中等[#550 Game Play Analysis IV](./#550 Game Play Analysis IV.md)    AVG函数应用
* 困难[#569 Median Employee Salary](./#569 Median Employee Salary.md)    SQL变量应用
* 困难[#579 Find Cumulative Salary of an Employee](./#579 Find Cumulative Salary of an Employee.md)    自连接查询近三个月的薪资和
* $\clubs$困难[#601 Human Traffic of Stadium](./#601 Human Traffic of Stadium.md)    复杂逻辑自连接查询
* 中等[#612 Shortest Distance in a Plane](./#612 Shortest Distance in a Plane.md)    自连接查询计算距离
* 中等[#626 Exchange Seats](./#626 Exchange Seats.md)    交换位置
* 简单[#1179 Reformat Department Table](./#1179 Reformat Department Table.md)    CASE语句行转列
* 中等[#1205 Monthly Transactions II](./#1205 Monthly Transactions II.md)    UNION ALL和DATE_FORMAT函数
* 中等[#1212 Team Scores in Football Tournament](./#1212 Team Scores in Football Tournament.md)    CASE WHEN语句统计分数
* 中等[#1285 Find the Start and End Number of Continuous Ranges](./#1285 Find the Start and End Number of Continuous Ranges.md)    SQL变量应用
* 困难[#1369 Get the Second Most Recent Activity](./#1369 Get the Second Most Recent Activity.md)    HAVING条件应用
* $\clubs$中等[#1454 Active Users](./#1454 Active Users.md)    连续时间复杂查询
* 中等[#1468 Calculate Salaries](./#1468 Calculate Salaries.md)    统计部门最大薪资并计算税率
* 中等[#1555 Bank Account Summary](./#1555 Bank Account Summary.md)    UNION ALL合并并统计
* 简单[#1607 Sellers With No Sales](./#1607 Sellers With No Sales.md)    LEFT JOIN的使用

## SHELL

* $\clubs$中等[#192 Word Frequency](./#192 Word Frequency.md)    `tr -s`替换分隔
* 简单[#193 Valid Phone Numbers](./#193 Valid Phone Numbers.md)    `grep -P`正则匹配
* $\bigstar$中等[#194 Transpose File](./#194 Transpose File.md)    `awk`函数编程
* 简单[#195 Tenth Line](./#195 Tenth Line.md)    `awk`命令`$0`整行打印

## 其它算法问题

### 一次线性扫描

* $\bigstar$困难[#335 Self Crossing](./#335 Self Crossing.md)    模拟螺旋
* 简单[#419 Battleships in a Board](./#419 Battleships in a Board.md)    一次遍历计数战列舰船首
* 简单[#463 Island Perimeter](./#463 Island Perimeter.md)    一次遍历计算岛屿周长
* 中等[#1109 Corporate Flight Bookings](./#1109 Corporate Flight Bookings.md)    区间求和

### 摩尔投票

* $\clubs$中等[#169 Majority Element](./#169 Majority Element.md)    摩尔投票寻找众数
* $\clubs$中等[#229 Majority Element II](./#229 Majority Element II.md)    [#169 Majority Element](./#169 Majority Element.md)进阶

### 荷兰旗问题



### 背包问题

&emsp;01背包问题为假设有$N$件物品和一个容量为$V$的背包。第$i$件物品的费用为$w_i$，价值为$v_i$，求装入背包使得总价值最大，其中每个物品只能使用1次。基本思想为`dp(i,j)`表示前$i$个物品放入$j$容量大小的背包的最大价值，则$dp(i,j) = max(dp(i-1,j), dp(i-1,j-w_i) + v_i)$，即当前最大值为不放入第$i$个物品和放入第$i$个物品后较大值。

```java
int[][] dp = new int[n + 1][c + 1];
// 若问题为恰好填满背包，则需要初始化dp(i,0)为0，表示前i个物品恰好填满背包，其他设定为负无穷大，表示暂时填不满
// ，更新只能在填满背包的基础上更新（负无穷大加有限的正数还是负数），这样最后得到的是敲好填满背包个的最大收益；
// 若问题是填入背包的最大收益，则全部初始为0，表示初始状态各个背包收益为0，可加入背包，更新可以在所有背包更新。
for (int i = 0; i <= n; i++) {
    Arrays.fill(dp[i], Integer.MIN_VALUE);
    dp[i][0] = 0;
}

for (int i = 1; i <= n && i <= w.length; i++) {
    for (int j = c; j >= 0; j--) {
        if (j < w[i - 1]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - w[i - 1]] + v[i - 1]);
    }
}
```

&emsp;当每件物品都有无限件时，就演变为完全背包问题。$dp(i,j) = max(dp(i-1,j-k*w_i) + k*v_i)$，其中$0 \le k*w_i \le j$。

```java
for (int i = 1; i <= n && i <= w.length; i++) {
    for (int j = c; j >= 0; j--) {
        dp[i][j] = dp[i - 1][j];
        for (int k = 1; k * w[i - 1] <= j; k++) {
            dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - k * w[i - 1]] + k * v[i - 1]);
        }
    }
}
```

可优化为一维数组，二层循环遍历方向为顺序，采取叠加的形式代替上面的第三个循环。

```java
for (int i = 1; i <= n && i <= w.length; i++) {
    for (int j = w[i - 1]; j <= c; j++) {
        dp[j] = Math.max(dp[j], dp[j - w[i - 1]] + v[i - 1]);
    }
}
```

&emsp;如果指定每个物品的使用次数为$M_i$，则转变为多重背包问题，转移方程不变：$dp(i,j) = max(dp(i-1,j-k*w_i) + k*v_i)$，只是条件为$0 \le k \le M_i$，基本形式修改第三层循环条件即可。

> 上述三种模式混合，则可在内部循环判断使用哪种转移方程。

&emsp;背包问题的变形，求排列组合的问题只需调换内外层循环即可，动态规划数组也要相应的变化。

* $\clubs$中等[#322 Coin Change](./#322 Coin Change.md)    完全背包问题变形的组合问题
* $\bigstar$中等[#377 Combination Sum IV](./#377 Combination Sum IV.md)    完全背包变形的排列问题
* $\clubs$中等[#416 Partition Equal Subset Sum](./#416 Partition Equal Subset Sum.md)    01背包问题变形
* $\clubs$中等[#474 Ones and Zeroes](./#474 Ones and Zeroes.md)    二维背包问题
* $\clubs$中等[#518 Coin Change 2](./#518 Coin Change 2.md)    完全背包问题变形的组合问题
* 中等[#1049 Last Stone Weight II](./#1049 Last Stone Weight II.md)    巧妙转化为背包问题
* $\bigstar$困难[#1449 Form Largest Integer With Digits That Add up to Target](./#1449 Form Largest Integer With Digits That Add up to Target.md)    完全背包问题内层循环优化
* 中等[#面试题08.11 Coin LCCI](./#面试题08.11 Coin LCCI.md)    [#322 Coin Change](./#322 Coin Change.md)

### 位图

```java
public class BitMap {
    // char类型占16bit
    private char[] bytes;
    private int nbits;
    
    public BitMap(int nbits) {
    	this.nbits = nbits;
    	this.bytes = new char[nbits / 16 + 1];
  	}

  	public void set(int k) {
    	if (k > nbits) return;
    	int byteIndex = k / 16;
    	int bitIndex = k % 16;
    	bytes[byteIndex] |= (1 << bitIndex);
  	}

  	public boolean get(int k) {
    	if (k > nbits) return false;
    	int byteIndex = k / 16;
    	int bitIndex = k % 16;
    	return (bytes[byteIndex] & (1 << bitIndex)) != 0;
  	}
}
```



### 数位动态规划

&emsp;todo

* $\clubs$困难[#1012 Numbers With Repeated Digits](./#1012 Numbers With Repeated Digits.md)    数位动态规划计算非重复数字（非最优）
* $\bigstar$技巧[#1397 Find All Good Strings](./#1397 Find All Good Strings.md)    状态为`KMP`匹配的数位动态规划