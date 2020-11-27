[toc]

There are $N$ gas stations along a circular route, where the amount of gas at station $i$ is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from station i to its next station ($i+1$). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return $-1$.



**Note**:

* If there exists a solution, it is guaranteed to be unique.
* Both input arrays are non-empty and have the same length.
* Each element in the input arrays is a non-negative integer.



## 题目解读

&emsp;给定加油站可加的油，和到达下一个加油站的花费，判断是否存在一个加油站作为起点可以循环，不存在则返回$-1$，存在则返回起点。

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {

    }
}
```

## 程序设计

* 如果存在循环线路，则必然石油总数大于等于花销总数；如果存在，如何判断起点？首先起点必然石油大于花销，以起点为开端的路线必然石油和大于花销；有了这个思路，可以从前向后遍历，记录每段路线的石油花销差，如果当前这段路线差为负数，表示达到不了后面的加油站，起点不在这段路径，重新定位起点，向后遍历。最后返回最后满足上述条件的起点。
* 这里需要继续讨论答案的有效性。如果总的石油消耗差为非负数，数组中存在多个序列`a`、`b`、`c`、……等满足序列石油差为非负数，假设最后一个满足条件的序列为`z`，由于前面的序列虽然各自满足差为非负数，但是算上其后的第一个石油站就会变成负数，不满足条件，即`z`前的所有石油站差为负数，又由于总的差为非负数，则以`z`为起始段必然满足条件。这样就证明了如果所有石油消耗差为非负数，必然存在一个起点。

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        if (gas == null || gas.length == 0) return -1;

        int idx = 0;
        // 石油与消耗的差值
        int totalDiff = 0, curDiff = 0;
        for (int i = 0; i < gas.length; i++) {
            totalDiff += gas[i] - cost[i];
            curDiff += gas[i] - cost[i];
            // i到达不了i+1，设置i+1为起点检测
            if (curDiff < 0) {
                idx = i + 1;
                curDiff = 0;
            }
        }

        return totalDiff >= 0 ? idx : -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.2MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;上述思路参考官方解题。