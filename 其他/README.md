[toc]

## 设计

* 简单[#157 Read N Characters Given Read4](./#157 Read N Characters Given Read4.md)    循环查询
* 困难[#158 Read N Characters Given Read4 II - Call multiple times](./#158 Read N Characters Given Read4 II - Call multiple times.md)    寄存查询剩余的结果
* 简单[#281 Zigzag Iterator](./#281 Zigzag Iterator.md)    锯齿迭代器
* 简单[#284 Peeking Iterator](./#284 Peeking Iterator.md)    设计支持预取的迭代器
* 简单[#348 Design Tic-Tac-Toe](./#348 Design Tic-Tac-Toe.md)    矩形行、列、对角线的表示
* 中等[#535 Encode and Decode TinyURL](./#535 Encode and Decode TinyURL.md)    短链系统设计
* $\clubs$困难[#843 Guess the Word](./#843 Guess the Word.md)    启发式极小极大算法

## 逻辑

* $\clubs$中等[#277 Find the Celebrity](./#277 Find the Celebrity.md)    查找名人
* $\clubs$中等[#681 Next Closest Time](./#681 Next Closest Time.md)    最近的时间
* 中等[#794 Valid Tic-Tac-Toe State](./#794 Valid Tic-Tac-Toe State.md)    井字棋是否有效验证

## 技巧

* $\clubs$中等[#54 Spiral Matrix](./#54 Spiral Matrix.md)    螺旋矩阵
* 中等[#59 Spiral Matrix II](./#59 Spiral Matrix II.md)    [#54 Spiral Matrix](./#54 Spiral Matrix.md)的变形
* $\clubs$中等[#73 Set Matrix Zeroes](./#73 Set Matrix Zeroes.md)    利用矩形空间做标识
* $\bigstar$中等[#152 Maximum Product Subarray](./#152 Maximum Product Subarray.md)    正向反向遍历计算最大值
* $\clubs$中等[#238 Product of Array Except Self](./#238 Product of Array Except Self.md)    前缀积
* 中等[#289 Game of Life](./#289 Game of Life.md)   新增状态巧妙利用空间
* $\clubs$中等[#304 Range Sum Query 2D - Immutable](./#304 Range Sum Query 2D - Immutable.md)    前缀和思想在二维数组的应用
* 简单[#724 Find Pivot Index](./#724 Find Pivot Index.md)    前缀和
* $\clubs$困难[#828 Count Unique Characters of All Substrings of a Given String](./#828 Count Unique Characters of All Substrings of a Given String.md)    计数每个字符不重复区间组合
* $\clubs$中等[#885 Spiral Matrix III](./#885 Spiral Matrix III.md)    [#54 Spiral Matrix](./#54 Spiral Matrix.md)的进阶，从内往外旋转
* 中等[#939 Minimum Area Rectangle](./#939 Minimum Area Rectangle.md)    拟对角线遍历验证
* 中等[#1292 Maximum Side Length of a Square with Sum Less than or Equal to Threshold](./#1292 Maximum Side Length of a Square with Sum Less than or Equal to Threshold.md)    矩形前缀和优化
* 简单[#1375 Bulb Switcher III](./#1375 Bulb Switcher III.md)    编号求和比较

## 多线程

* 简单[#1114 Print in Order](./#1114 Print in Order.md)    线程间变量共享通信
* 中等[#1115 Print FooBar Alternately](./#1115 Print FooBar Alternately.md)    `ReentrantLock`线程互相唤醒

## 数据库

* 简单[#175 Combine Two Tables](./#175 Combine Two Tables.md)    LEFT JOIN语句
* 简单[#176 Second Highest Salary](./#176 Second Highest Salary.md)    LIMIT和OFFSET的组合
* 简单[#182 Duplicate Emails](./#182 Duplicate Emails.md)    GROUP BY和HAVING的组合
* 中等[#184 Department Highest Salary](./#184 Department Highest Salary.md)    IN技巧

## SHELL

* $\clubs$中等[#192 Word Frequency](./#192 Word Frequency.md)    `tr -s`替换分隔
* 简单[#193 Valid Phone Numbers](./#193 Valid Phone Numbers.md)    `grep -P`正则匹配

## 其它算法问题

### 一次线性扫描

* 简单[#419 Battleships in a Board](./#419 Battleships in a Board.md)    一次遍历计数战列舰船首
* 简单[#463 Island Perimeter](./#463 Island Perimeter.md)    一次遍历计算岛屿周长

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

* $\clubs$中等[#416 Partition Equal Subset Sum](./#416 Partition Equal Subset Sum.md)    01背包问题变形
* $\clubs$中等[#518 Coin Change 2](./#518 Coin Change 2.md)    完全背包问题变形

### 位图

```java

public class BitMap { // Java中char类型占16bit，也即是2个字节
  private char[] bytes;
  private int nbits;
  
  public BitMap(int nbits) {
    this.nbits = nbits;
    this.bytes = new char[nbits/16+1];
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

todo 布隆过滤器