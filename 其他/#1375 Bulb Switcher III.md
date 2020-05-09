[toc]

There is a room with `n` bulbs, numbered from `1` to `n`, arranged in a row from left to right. Initially, all the bulbs are turned off.

At moment `k` (for `k` from `0` to `n - 1`), we turn on the `light[k]` bulb. A bulb **change color to blue** only if it is on and all the previous bulbs (to the left) are turned on too.

Return the number of moments in which **all turned on** bulbs **are blue**.



Constraints:

* $n == \text{light.length}$
* $1 \le n \le 5 * 10^4$
* `light` is a permutation of  `[1, 2, ..., n]`



## 题目解读

&emsp;每个时刻开一盏灯，只有之前的灯都亮时，当前灯变为蓝色。求灯全为蓝色的时刻数目。

```java
class Solution {
    public int numTimesAllBlue(int[] light) {

    }
}
```

## 程序设计

* 当前时刻灯全是蓝色时，灯的编号从$1$到$k$是连续的，可以记录求和当前时刻连续序列的和，比较截止当前时刻灯的编号之和，相等则说明是连续的。

```java
class Solution {
    public int numTimesAllBlue(int[] light) {
        int sum1 = 0, sum2 = 0;
        int count = 0;
        for (int i = 0; i < light.length; i++) {
            // 连续序列和
            sum1 += i + 1;
            // 灯编号和
            sum2 += light[i];
            // 连续
            if (sum1 == sum2)count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了99.12%的用户。

内存消耗：48.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。