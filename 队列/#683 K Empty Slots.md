[toc]

You have `N` bulbs in a row numbered from `1` to `N`. Initially, all the bulbs are turned off. We turn on exactly one bulb everyday until all bulbs are on after `N` days.

You are given an array `bulbs` of length `N` where `bulbs[i] = x` means that on the `(i+1)th` day, we will turn on the bulb at position `x` where `i` is `0-indexed` and `x` is `1-indexed`.

Given an integer `K`, find out the **minimum day number** such that there exists two **turned on** bulbs that have **exactly** `K` bulbs between them that are **all turned off**.

If there isn't such day, return `-1`.



**Note**:

* $1 \le N \le 20000$
* $1 \le \text{bulbs[i]} \le N$
* `bulbs` is a permutation of numbers from `1` to `N`.
* $0 \le K \le 20000$



## 题目解读

&emsp;给定花，计算最小的天数使得两朵花之间正好有`k`朵花未开。

```java
class Solution {
    public int kEmptySlots(int[] bulbs, int K) {

    }
}
```

## 程序设计

* 最基本的思路使用红黑树，每次遍历查找当前位置的前一朵花与后一朵花，比对距离是否满足要求。

```java
class Solution {
    public int kEmptySlots(int[] bulbs, int K) {
        if (bulbs == null || bulbs.length < K + 2) return -1;

        int n = bulbs.length;
        TreeSet<Integer> set = new TreeSet<>();
        for (int i = 0; i < n; i++) {
            int pos = bulbs[i];
            Integer pre = set.floor(pos);
            if (pre != null && pos - pre - 1 == K) return i + 1;
            Integer next = set.ceiling(pos);
            if (next != null && next - pos - 1 == K) return i + 1;
            set.add(pos);
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：80ms，在所有java提交中击败了26.67%的用户。

内存消耗：47.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了一次线性遍历的思路，即首先将天数-位置数组转化为位置-天数数组，则问题转化为在某个长度为`k`的区间内天数都大于两侧端点的天数的问题。起始区间为$0 \sim K + 1$，在这之间遍历判断是否大于两侧端点，如果全都大于两侧端点，说明区间符合条件，更新天数；如果存在小于两侧端点的位置，则重新定位区间，以该点为新的左侧端点并更新右侧端点，继续重复上述步骤。

```java
class Solution {
    public int kEmptySlots(int[] bulbs, int K) {
        if (bulbs == null || bulbs.length < K + 2) return -1;

        int n = bulbs.length;
        int[] days = new int[n];
        for (int i = 0; i < n; i++) days[bulbs[i] - 1] = i + 1;

        int minDay = Integer.MAX_VALUE;
        int left = 0, right = K + 1;
        int idx = left + 1;
        while (right < n) {
            while (idx < right && days[idx] > days[left] && days[idx] > days[right]) idx++;
            // 区间内符合要求，记录天数
            if (idx == right) minDay = Math.min(minDay, Math.max(days[left], days[right]));
            // 边界重新定位到idx，如果当前区间符合条件，则当前区间内时间都大于左右端点，
            // 若尝试区间内为边界的情况必然得到结果天数大于当前区间，故可不再尝试；若区间不符合条件，
            // 此时idx处天数小于左右端点，作为新的左端点
            left = idx;
            right = left + K + 1;
            idx++;
        }
        return minDay == Integer.MAX_VALUE ? -1 : minDay;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了97.33%的用户。

内存消耗：47.2MB，在所有java提交中击败了100.00%的用户。