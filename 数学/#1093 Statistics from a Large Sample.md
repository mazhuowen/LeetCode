[toc]

We sampled integers between `0` and `255`, and stored the results in an array `count:  count[k]` is the number of integers we sampled equal to `k`.

Return the minimum, maximum, mean, median, and mode of the sample respectively, as an array of **floating point numbers**.  The mode is guaranteed to be unique.

(Recall that the median of a sample is:

* The middle element, if the elements of the sample were sorted and the number of elements is odd;
* The average of the middle two elements, if the elements of the sample were sorted and the number of elements is even.)



**Constraints**:

* $\text{count.length} == 256$
* $1 \le \text{sum(count)} \le 10^9$
* The mode of the sample that count represents is unique.
* Answers within $10^{-5}$ of the true value will be accepted as correct.



## 题目解读

&emsp;对区间$0 \sim 255$得数字进行采样，将采样结果放入数组中，数组索引表示数字，值表示采样到的该数字的数目，求最小、最大、均值、中位数、众数。

```java
class Solution {
    public double[] sampleStats(int[] count) {
        
    }
}
```

## 程序设计

* 难点主要在于中位数的统计，需要先统计出总数，然后再遍历判断。

```java
class Solution {
    public double[] sampleStats(int[] count) {
        if (count == null) throw new IllegalArgumentException("invalid param");
        double[] res = new double[]{Double.MAX_VALUE, 0.0, 0.0, Double.MAX_VALUE, 0.0};
        long sum = 0, counter = 0, maxCount = 0;
        for (int num = 0; num < count.length; num++) {
            int c = count[num];
            if (c == 0) continue;

            sum += num * c;
            counter += c;
            // 最小值
            if (res[0] == Double.MAX_VALUE) res[0] = num;
            // 最大值
            res[1] = num;
            // 众数
            if (c > maxCount) {
                maxCount = c;
                res[4] = num;
            }
        }
        // 平均值
        if (counter != 0) res[2] = (double)sum / counter;

        // 统计中位数
        int time = 0;
        for (int num = 0; num < count.length; num++) {
            int c = count[num];
            if (c == 0) continue;

            time += c;
            if (time > counter / 2 || counter % 2 == 1 && time == counter / 2) {
                if (counter % 2 == 0 && res[3] != Double.MAX_VALUE) res[3] = (res[3] + num) / 2;
                else res[3] = num;
                break;
            }
            if (time == counter / 2) res[3] = num;
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了94.07%的用户。

内存消耗：40.4MB，在所有java提交中击败了43.75%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有双指针的思路，只需一次遍历。