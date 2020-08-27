[toc]

On a horizontal number line, we have gas stations at positions `stations[0], stations[1], ..., stations[N-1]`, where $N = \text{stations.length}$.

Now, we add $K$ more gas stations so that **D**, the maximum distance between adjacent gas stations, is minimized.

Return the smallest possible value of **D**.



**Note**:

* $\text{stations.length}$ will be an integer in range $[10, 2000]$.
* $\text{stations[i]}$ will be an integer in range $[0, 10^8]$.
* $K$ will be an integer in range $[1, 10^6]$.
* Answers within $10^{-6}$ of the true value will be accepted as correct.



## 题目解读

&emsp;在一条直线上有$N$个加油站，现在最多添加$K$个加油站，使得加油站两两之间距离最小，返回最小的距离。

```java
class Solution {
    public double minmaxGasDist(int[] stations, int K) {

    }
}
```

## 程序设计

* 参考官方思路，采用二分查找遍历尝试**D**。关键在于确定**D**后的所需加油站计算，官方采用`(stations[i + 1] - stations[i]) / D`的形式来计算两个加油站之间满足距离**D**时所需额外加入的加油站；假设为`(2 - 1) / 0.5`，需要在$1$和$2$中间插入一个加油站，使得距离为$0.5$，但是根据公式计算出来是$2$，似乎不对，事实上二分查找由于是`double`类型，得到的**D**不会正好是$0.5$，可能是$0.50001$或$0.49996$，又由于公式的性质，得到的**D**会是较大的结果，即$0.50001$。

```java
class Solution {
    public double minmaxGasDist(int[] stations, int K) {
        int n = stations.length;
        double left = 0.0D, right = stations[n - 1];
        while (right - left > 1e-6) {
            double mid = (left + right) / 2;
            if (possible(mid, stations, K)) right = mid;
            else left = mid;
        }
        return left;
    }

    private boolean possible(double dis, int[] stations, int K) {
        for (int i = 0; i < stations.length - 1; i++) {
            K -= (int)((stations[i + 1] - stations[i]) / dis);
        }
        return K >= 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2M)$，空间复杂度为$O(1)$。

执行用时：18ms，在所有 Java 提交中击败了90.32%的用户。

内存消耗：40.4MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;见上。