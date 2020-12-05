[toc]

Given an array of `prices` `[p1,p2...,pn]` and a `target`, round each price `pi` to `Roundi(pi)` so that the rounded array `[Round1(p1),Round2(p2)...,Roundn(pn)]` sums to the given `target`. Each operation `Roundi(pi)` could be either `Floor(pi)` or `Ceil(pi)`.

Return the string `"-1"` if the rounded array is impossible to sum to `target`. Otherwise, return the smallest rounding error, which is defined as $\sum \vert \text{Round}_i(p_i) - (p_i)\rvert$ for $i$ from $1$ to $n$, as a string with three places after the decimal.

 

**Example 1**:

```
Input: prices = ["0.700","2.800","4.900"], target = 8
Output: "1.000"
Explanation:
Use Floor, Ceil and Ceil operations to get (0.7 - 0) + (3 - 2.8) + (5 - 4.9) = 0.7 + 0.2 + 0.1 = 1.0 .
```

**Example 2**:

```
Input: prices = ["1.500","2.500","3.500"], target = 10
Output: "-1"
Explanation: It is impossible to meet the target.
```

**Example 3**:

```
Input: prices = ["1.500","2.500","3.500"], target = 9
Output: "1.500"
```



**Constraints**:

* $1 \le \text{prices.length} \le 500$
* Each string `prices[i]` represents a real number in the range $[0.0, 1000.0]$ and has exactly $3$ decimal places.
* $0 \le \text{target} \le 10^6$



## 题目解读

&emsp;给定数组和目标值，判断将数组中每个元素向上舍入或向下舍入后数组和是否等于目标值，如果存在方案，求误差最小的方案。误差定义为舍入与原数字的幅度。

```java
class Solution {
    public String minimizeError(String[] prices, int target) {

    }
}
```

## 程序设计

* 首先数组给定可可以计算数组四舍五入后的区间，如果目标值不在区间中，则无法得到；
* 问题在于如何得到最小误差，由于数字向下舍入和向上舍入最多差距是$1$，这样可以用目标值减去下限得到需要向上舍入的数字数目；可以提前计算出数字全部向下舍入的误差，然后在数组中维护当前数字向上舍入误差与向下舍入误差之差，这样只需排序选择指定数目的最小该变量即可。

```java
class Solution {
    public String minimizeError(String[] prices, int target) {
        // 向上舍入的误差和向下舍入的误差之差
        double[] diff = new double[prices.length];
        // 向下舍入误差
        double error = 0D;
        // 可得的数字范围
        int low = 0, high = 0;
        for (int i = 0; i < prices.length; i++) {
            double num = Double.parseDouble(prices[i]);
            int floor = floor(num), ceil = ceil(num);
            low += floor;
            high += ceil;
            error += num - floor;
            // 向上舍入与向下舍入误差之差
            diff[i] = ceil + floor - 2 * num;
        }

        // 无法得到目标值
        if (target < low || target > high) return "-1";

        // 获得需要向上四舍五入的数目
        int count = target - low;
        Arrays.sort(diff);

        // 获取count个最小的改变量
        for (int i = 0; i < count; i++) {
            error += diff[i];
        }
        // 需保存后三位字符串格式
        DecimalFormat df = new DecimalFormat("0.000");
        return df.format(error);
    }

    private int floor(double num) {
        return (int)num;
    }

    private int ceil(double num) {
        int ceil = floor(num);
        if (num > ceil) ceil++;
        return ceil;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：5 ms, 在所有 Java 提交中击败了94.59%的用户。

内存消耗：37.7 MB, 在所有 Java 提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。