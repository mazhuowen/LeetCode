[toc]

Koko loves to eat bananas.  There are `N` piles of bananas, the i-th pile has `piles[i]` bananas.  The guards have gone and will come back in `H` hours.

Koko can decide her bananas-per-hour eating speed of `K`.  Each hour, she chooses some pile of bananas, and eats `K` bananas from that pile.  If the pile has less than `K` bananas, she eats all of them instead, and won't eat any more bananas during this hour.

Koko likes to eat slowly, but still wants to finish eating all the bananas before the guards come back.

Return the minimum integer `K` such that she can eat all the bananas within `H` hours.

Note:

* $1 \le \text{piles.length} \le 10^4$
* $\text{piles.length} \le H \le 10^9$
* $1 \le \text{piles[i]} \le 10^9$



## 题目解读

&emsp;珂珂喜欢吃香蕉。有`N`堆香蕉，第`i`堆中有`piles[i]`根香蕉。警卫已经离开了，将在`H`小时后回来。珂珂可以决定她吃香蕉的速度`K`（单位：根/小时）。每个小时她将会选择一堆香蕉，从中吃掉`K`根。如果这堆香蕉少于`K`根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。返回她可以在`H`小时内吃掉所有香蕉的最小速度`K`（`K`为整数）。

```java
class Solution {
    public int minEatingSpeed(int[] piles, int H) {

    }
}
```

## 程序设计

* 首先`H`不能小于堆的数目，当`H`等于堆的数目时，最小的吞食速度就是最大的那堆香蕉数目；当`H`大于堆的数目时，需要假设吞食速度为1～最大食物堆数目，在这个区间遍历。
* 遍历可以采用二分查找，每次统计时间并做下一步操作。

```java
class Solution {
    public int minEatingSpeed(int[] piles, int H) {
        if (piles == null || piles.length == 0) return 0;
        // 最小和最大的吞食速度（最大吞食速度为最大堆的尺寸）
        int left = 1, right = -1;
        // 统计最大尺寸
        for (int num : piles) right = Math.max(num, right);
        if (H == piles.length) return right;
        // 二分查找统计
        while (left < right) {
            int mid = left + (right - left) / 2;
            int count = 0;
            for (int i = 0; i < piles.length; i++) {
                // 吃完一堆香蕉的时间，如果有余则时间加一
                count += piles[i] % mid == 0 ? piles[i] / mid : piles[i] / mid + 1;
            }
            // 如果当前速度mid可以在H内吃完，继续向前遍历探索更小的值
            if (count <= H) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        // 返回
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂福为$O(N\log_2max)$，空间复杂度为$O(1)$，其中$max$是数组中最大值。

执行用时：22ms，在所有java提交中击败了34.71%的用户。

内存消耗：41.8MB，在所有java提交中击败了5.13%的用户。

## 官方解题

&emsp;同上。