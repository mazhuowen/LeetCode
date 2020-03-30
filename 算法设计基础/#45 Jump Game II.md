[toc]

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

**Note:**

You can assume that you can always reach the last index.



## 题目解读

&emsp;[#55 Jump Game](./#55 Jump Game.md)的延伸，需要求解最短路径。

```java
class Solution {
    public int jump(int[] nums) {
        
    }
}
```

## 程序设计

* 题目限定必然存在可达路径，可以从数组尾部向前寻找最前面可达终点的点，然后将其作为新的目标点继续向前遍历寻找。假设能到达目标点`a`的最前面的点为`b`，如果起点可达`b`则`b`无疑是最佳路径结点；如果`b`不可达起点，`a`到`b`之间某个点必然可达起点，不然与题意矛盾，假设为`c`，若`c`的最远可达前驱为`d`，则`d`必然在`b`前面，因为若`d`在`b`后面，则可达`d`的点距离必然可达`b`，从而`b`与起点不可达的假设矛盾。
* 通过上述分析，不管任何情况，当前最佳路径点必然在最前边可达当前目标点的点`b`到当前点`a`的位置之间，而前一个最佳路径点必然在`b`前面。这样可得到从后往前遍历，内层从前往后寻找第一个可达当前点的点，总共两层循环，
* 仔细梳理上述逻辑，可以直接从前往后遍历，第一个最佳路径必然在起始点`a`和起始点可达的范围`b`间，第二个最佳路径则在`b`到起始点的所有后继位置可达的最远范围`c`之间，这样就大大缩短了搜索范围，依次类推。得到算法如下：

```java
class Solution {
    public int jump(int[] nums) {
        if (nums == null || nums.length == 0) return -1;
        // 步数
        int count = 0;
        // 记录后继位置能到达的最远距离，当前位置能到达的最远位置
        int maxDis = 0, boudary = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            maxDis = Math.max(maxDis, nums[i] + i);
            // 当前位置到达边界，继续下一个位置
            if (i == boudary) {
                count++;
                boudary = maxDis;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了94.90%的用户。

内存消耗：41.8MB，在所有java提交中击败了5.11%的用户。

## 官方解题

&emsp;暂无，密切关注。