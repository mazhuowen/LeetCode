[toc]

You are playing the following `Nim Game` with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.



## 题目解读

&emsp;`Nim`游戏。

```java
class Solution {
    public boolean canWinNim(int n) {

    }
}
```

## 程序设计

* 如果石头堆中只有一块、两块、或是三块石头，那么在我方回合可以把全部石子拿走，从而在游戏中取胜。堆中恰好有四块石头，就会失败。因为在这种情况下不管取走多少石头，总会为对手留下几块，使得他可以在游戏中获胜。因此要想获胜，在当前回合中，必须避免石头堆中的石子数为4的情况。

```java
class Solution {
    public boolean canWinNim(int n) {
        return (n & 3) != 0;
    }
}
```

> 原生的`Nim`游戏涉及博弈论和拓扑排序，有多种扩展，但是本题较简单，纯属规律发现。首先当石头数目为$0$表示败态$0$，假设每次能拿$m$个石头，则$SG(m) = m$，若有$m + 1$个石头，则$SG(m + 1) = SG(1) \oplus SG(2) \oplus \cdots \oplus SG(m) = 1 \oplus 2 \oplus \cdots \oplus m$，$SG(m + 2) = SG(m + 1) \oplus SG(1) \oplus SG(m + 1)$，每次都是出队之前的数值，入队当前的数值，即对于任意$n$，$SG(n) = SG(n - 1) \oplus SG(n - m -1) \oplus SG(n - 1) = SG(n - m - 1)$，可见以$m$为周期循环，由于我们只关注是否胜利，则只需当前$SG(n) \% m = n \% m \ne 0$即可。

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了5.08%的用户。

## 官方解题

&emsp;同上。