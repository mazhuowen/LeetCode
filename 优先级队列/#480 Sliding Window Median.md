[toc]

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:
`[2,3,4]` , the median is $3$

`[2,3]`, the median is $(2 + 3) / 2 = 2.5$

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.



Note:
You may assume `k` is always valid, ie: `k` is always smaller than input array's size for non-empty array.
Answers within $10^{-5}$ of the actual value will be accepted as correct.



## 题目解读

&emsp;给定数组，返回滑动窗口中的中位数。

```java
class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {

    }
}
```

## 程序设计

* 类似与[#295 Find Median from Data Stream](./#295 Find Median from Data Stream.md)，使用双堆来维护前半部分和后半部分。但本题的难点在于只能返回窗口内的中位数，由于进入堆之后无法判断需要出队的值在哪个堆中，导致无法维护两个堆的尺寸关系。
* 针对上述问题，引入哈希表记录数组中每个元素在哪个堆中，这样随着窗口滑动，减少相应堆的尺寸；其次堆是延迟删除，划出窗口的数值不会删除，直到下次两个堆互相操作才会删除在窗口外的值。
* 最后需要注意数值溢出，不管是堆的比较函数还是计算中位数，都需要转为长整型。

```java
class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length < k) throw new IllegalArgumentException("invalid param");

        int n = nums.length;
        double[] res = new double[n - k + 1];
        // 两个堆的尺寸
        int preSize = 0, nextSize = 0;
        // 最大堆
        PriorityQueue<Integer> prev = new PriorityQueue<>((a, b) -> {
            long diff = (long)nums[b] - nums[a];
            return diff > 0 ? 1 : -1;
        });
        // 最小堆
        PriorityQueue<Integer> next = new PriorityQueue<>((a, b) -> {
            long diff = (long)nums[a] - nums[b];
            return diff > 0 ? 1 : -1;
        });
        // 记录索引对应数字在哪个堆中
        Map<Integer, Integer> record = new HashMap<>();

        // 定位窗口
        int start = 0, end = 0;
        while (end < k - 1) {
            next.add(end);
            record.put(end, 2);
            end++;
            nextSize++;
        }
        while (preSize < nextSize) {
            int idx = next.poll();
            prev.add(idx);
            record.put(idx, 1);
            preSize++;
            nextSize--;
        }

        // 滑动窗口
        while (end < n) {
            // 延迟删除堆顶在窗口外的元素
            delayDelete(prev, next, start);

            next.add(end);
            record.put(end, 2);
            nextSize++;

            //　保持前半部分大于等于后半部分
            while (preSize < nextSize) {
                int idx = next.poll();
                prev.add(idx);
                record.put(idx, 1);
                preSize++;
                nextSize--;
                delayDelete(prev, next, start);
            }

            // 保持前半部分的值小于等于后半部分
            while (nextSize > 0 && nums[prev.peek()] > nums[next.peek()]) {
                int p = prev.poll(), s = next.poll();
                prev.add(s);
                next.add(p);
                record.put(s, 1);
                record.put(p, 2);
                delayDelete(prev, next, start);
            }

            // 根据奇偶数计算中位数
            if (k % 2 == 0) res[start] = ((long)nums[prev.peek()] + nums[next.peek()]) / 2.0;
            else res[start] = nums[prev.peek()];

            // 对于滑出窗口的值根据字典保存的位置，对相应的尺寸修改
            if (record.get(start) == 1) preSize--;
            else nextSize--;

            // 滑动
            start++;
            end++;
        }

        return res;
    }

    // 延迟删除
    private void delayDelete(PriorityQueue<Integer> prev, PriorityQueue<Integer> next, int start) {
        while (!prev.isEmpty() && prev.peek() < start) prev.poll();
        while (!next.isEmpty() && next.peek() < start) next.poll();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2K)$，空间复杂度为$O(N)$。

执行用时：35ms，在所有java提交中击败了90.63%的用户。

内存消耗：42.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;思路同上，实现细节不同。