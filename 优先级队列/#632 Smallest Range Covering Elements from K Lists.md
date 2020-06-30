[toc]

You have `k` lists of sorted integers in ascending order. Find the **smallest** range that includes at least one number from each of the `k` lists.

We define the range `[a,b]` is smaller than range [c,d] if $b-a < d-c$ or $a < c$ if $b-a == d-c$.



**Note**:

* The given list may contain duplicates, so ascending order means `>=` here.
* $1 \le k \le 3500$
* $-10^5 \le \text{value of elements} \le 10^5$.



## 题目解读

&emsp;给定`k`个有序链表，查找使得每个链表都至少包含一个值的最小区间。

```java
class Solution {
    public int[] smallestRange(List<List<Integer>> nums) {

    }
}
```

## 程序设计

* 最基本的思路是同时遍历`K`个链表，统计当前`K`个数字的大小得到区间。可使用堆来动态维护大小关系。当进行下次迭代时，只需出队最小值然后继续遍历入队该最小值的后继值，重复上述操作；如果当前最小值已经是其链表的最后一个元素，则结束查找。

```java
class Solution {
    public int[] smallestRange(List<List<Integer>> nums) {
        if (nums == null || nums.size() == 0) throw new IllegalArgumentException("invalid param");
        int k = nums.size();
        // 当前最大值
        int min, max = Integer.MIN_VALUE;
        // 最小堆
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < k; i++) {
            List<Integer> num = nums.get(i);
            if (num == null || num.isEmpty()) continue;

            // 入队当前值，所在链表id，链表中位置
            queue.add(new int[]{num.get(0), i, 0});
            max = Math.max(max, num.get(0));
        }

        if (queue.isEmpty())  throw new IllegalArgumentException("invalid param");
        min = queue.peek()[0];
        int[] res = new int[]{min, max};
        while (!queue.isEmpty()) {
            // 将最小值向后迭代
            int[] cur = queue.poll();
            // 如果最小值是链表最后一个元素，则返回
            if (cur[2] == nums.get(cur[1]).size() - 1) break;

            queue.add(new int[]{nums.get(cur[1]).get(cur[2] + 1), cur[1], cur[2] + 1});
            int len = res[1] - res[0];
            min = queue.peek()[0];
            max = Math.max(max, nums.get(cur[1]).get(cur[2] + 1));
            // 更新当前区间（由于是从小到大遍历，长度相等，之前的区间序号更小）
            if (max - min < len) {
                res[0] = min;
                res[1] = max;
            }
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2K)$，空间复杂度为$O(K)$。

执行用时：47ms，在所有java提交中击败了69.23%的用户。

内存消耗：45.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路类似。