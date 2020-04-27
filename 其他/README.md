[toc]

## 设计

* 简单[#348 Design Tic-Tac-Toe](./#348 Design Tic-Tac-Toe.md)    矩形行、列、对角线的表示
* 中等[#535 Encode and Decode TinyURL](./#535 Encode and Decode TinyURL.md)    短链系统设计

## 逻辑

* $\clubs$中等[#277 Find the Celebrity](./#277 Find the Celebrity.md)    查找名人
* 中等[#794 Valid Tic-Tac-Toe State](./#794 Valid Tic-Tac-Toe State.md)    井字棋是否有效验证

## 技巧

* $\clubs$中等[#54 Spiral Matrix](./#54 Spiral Matrix.md)    螺旋矩阵
* 中等[#59 Spiral Matrix II](./#59 Spiral Matrix II.md)    [#54 Spiral Matrix](./#54 Spiral Matrix.md)的变形
* $\clubs$中等[#73 Set Matrix Zeroes](./#73 Set Matrix Zeroes.md)    利用矩形空间做标识
* $\bigstar$中等[#152 Maximum Product Subarray](./#152 Maximum Product Subarray.md)    正向反向遍历计算最大值
* $\clubs$中等[#238 Product of Array Except Self](./#238 Product of Array Except Self.md)    前缀积
* 中等[#289 Game of Life](./#289 Game of Life.md)   新增状态巧妙利用空间
* $\clubs$中等[#304 Range Sum Query 2D - Immutable](./#304 Range Sum Query 2D - Immutable.md)    前缀和思想在二维数组的应用

## 其它算法问题

### 一次线性扫描

* 简单[#419 Battleships in a Board](./#419 Battleships in a Board.md)    一次遍历计数战列舰船首

### 摩尔投票

* $\clubs$中等[#169 Majority Element](./#169 Majority Element.md)    摩尔投票寻找众数
* $\clubs$中等[#229 Majority Element II](./#229 Majority Element II.md)    [#169 Majority Element](./#169 Majority Element.md)进阶

### 荷兰旗问题



### 背包问题

&emsp;01背包问题为假设有$N$件物品和一个容量为$V$的背包。第$i$件物品的费用为$w_i$，价值为$v_i$，求装入背包使得总价值最大，其中每个物品只能使用1次。基本思想为`dp(i,j)`表示前$i$个物品放入$j$容量大小的背包的最大价值，则$dp(i,j) = max(dp(i-1,j), dp(i-1,j-w_i) + v_i)$，即当前最大值为不放入第$i$个物品和放入第$i$个物品后较大值。

* $\clubs$中等[#416 Partition Equal Subset Sum](./#416 Partition Equal Subset Sum.md)    01背包问题变形
* todo 518