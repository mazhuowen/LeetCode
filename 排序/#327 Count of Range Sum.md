[toc]

Given an integer array `nums`, return the number of range sums that lie in `[lower, upper]` inclusive.
Range sum `S(i, j)` is defined as the sum of the elements in `nums` between indices $i$ and $j$ ($i \le j$), inclusive.



**Note:**
A naive algorithm of $O(n^2)$ is trivial. You MUST do better than that.



## 题目解读

&emsp;给定数组，返回区间和在给定范围内的数目。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        
    }
}
```

## 程序设计

* 最基本的思路计算前缀和，然后两层循环遍历计算。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        if(nums.length == 0) {
            return 0;
        }
        // 注意溢出，采用长整形
        long[] preSum = new long[nums.length + 1];
        for(int i = 0; i < nums.length; i++) {
            preSum[i + 1] = nums[i] + preSum[i];
        }
        int count = 0;
        for(int i = 1; i <= nums.length; i++) {
            for(int j = 0; j < i; j++) {
                // 采用长整形
                long sum = preSum[i] - preSum[j];
                if(sum >= lower && sum <= upper) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

* 根据暴力法的思路，得到前缀和后不两次循环扫描，而是分治计算。但分治计算仍然需要遍历平方次。可以使用归并排序，这样左右区间是有序的，可以使用线性扫描来计算满足条件的区间值。
* 归并阶段左右区间有序，左、右区间已统计出满足条件的区间数，现在需要统计当前区间满足条件的数目，也就是左区间跨到右区间满足条件的数目。可以利用区间的有序性，选取左区间索引`i`，右区间需要找到满足条件的前缀和区间`l`和`h`，此时数目就是`h - l`；接着遍历下一个索引`i + 1`，由于序列是有序的，左序列和增大则总的区间和减少，`l`和`h`可以继续从当前位置遍历，而不需要回到起始位置，这样整个扫描是线性的。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        // 计算前缀和（注意溢出）
        long[] preSum = new long[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
        }
        // 对前缀和归并并计算区间距离（注意前缀和不包含0）
        int count = mergeCount(preSum, lower, upper, 1, nums.length);
        return count;
    }

    private int mergeCount(long[] nums, int lower, int upper, int start, int end) {
        if (start > end) {
            return 0;
        } else if (start == end) {
            // 由于排序的前缀和不包含0，无法通过减0得到整段序列的和，需要对每个值进行判断
            return nums[start] >= lower && nums[end] <= upper ? 1 : 0;
        }
        int mid =  start + (end - start) / 2;
        // 左右区间满足条件的值
        int count = mergeCount(nums, lower, upper, start, mid) + mergeCount(nums, lower, upper, mid + 1, end);
        int l = mid + 1;
        int h = mid + 1;
        // 遍历当前区间（由于左右区间是有序的，扫描为线性）
        for (int i = start; i <= mid; i++) {
            while (l <= end && nums[l] - nums[i] < lower) {
                l++;
            }
            while (h <= end && nums[h] - nums[i] <= upper) {
                h++;
            }
            // i对应的值满足条件的区间数
            count += h - l;
        }
        mergeSort(nums, start, mid, end);
        return count;
    }
    // 归并排序流程
    private void mergeSort(long[] nums, int start, int mid, int end) {
        long[] temp = new long[end - start + 1];
        int i = start, j = mid + 1, idx = 0;
        // 归并
        while (i <= mid && j <= end) {
            if (nums[i] <= nums[j]) {
                temp[idx++] = nums[i++];
            } else {
                temp[idx++] = nums[j++];
            }
        }
        while (i <= mid) {
            temp[idx++] = nums[i++];
        }
        while (j <= end) {
            temp[idx++] = nums[j++];
        }
        // 复制回原数组
        for (int k = 0; k < temp.length; k++) {
            nums[start + k] = temp[k];
        }
    }
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：190ms，在所有java提交中击败了8.48%的用户。

内存消耗：41MB，在所有java提交中击败了5.95%的用户。

&emsp;归并法时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：8ms, 在所有java提交中击败了95.96%的用户。

内存消耗：42.4MB，在所有java提交中击败了5.48%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了基于线段树和树状数组的思路。由于`preSum[j]-preSum[j]`$\in [lower, upper]$，可转化为为统计$[preSum[j] - upper, preSum[j] - lower]$区间内点的数目。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        long min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        long[] preSum = new long[n + 1];
        for (int i = 0; i < n; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
            min = Math.min(min, preSum[i + 1]);
            max = Math.max(max, preSum[i + 1]);
        }

        int res = 0;
        SegmentTree tree = new SegmentTree(min, max);
        for (int i = 0; i < n; i++) {
            if (preSum[i + 1] >= lower && preSum[i + 1] <= upper) res++;
            res += tree.query(preSum[i + 1] - upper, preSum[i + 1] - lower);
            tree.update(preSum[i + 1], 1);
        }
        return res;
    }
}

class SegmentTree {
    long start, end;
    int count;
    SegmentTree left, right;

    SegmentTree(long start, long end) {
        this.start = start;
        this.end = end;
    }

    private long mid() {
        return start + (end - start) / 2;
    }

    private SegmentTree left() {
        if (this.left == null) this.left = new SegmentTree(start, mid());
        return this.left;
    }

    private SegmentTree right() {
        if (this.right == null) this.right = new SegmentTree(mid() + 1, end);
        return this.right;
    }

    public int query(long s, long e) {
        if (s > e || this.start > e || this.end < s) return 0;
        if (s <= this.start && e >= this.end) return this.count;
        else return this.left().query(s, e) + this.right().query(s, e);
    }

    public void update(long idx, int val) {
        if (this.start > idx || this.end < idx) return;
        this.count += val;
        if (this.start == this.end) return;
        this.left().update(idx, val);
        this.right().update(idx, val);
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：16 ms, 在所有 Java 提交中击败了54.62%的用户。

内存消耗：39 MB, 在所有 Java 提交中击败了16.00%的用户。

&emsp;也可使用树状数组实现。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        long[] preSum = new long[n];
        preSum[0] = nums[0];
        for (int i = 1; i < n; i++) preSum[i] = preSum[i - 1] + nums[i];

        // 离散化
        long[] tmp = Arrays.copyOf(preSum, n);
        Arrays.sort(tmp);
        TreeMap<Long, Integer> index = new TreeMap<>();
        // 编号
        int idx = 0;
        for (int i = 0; i < n; i++) {
            if (!index.containsKey(tmp[i])) index.put(tmp[i], idx++);
        }

        int res = 0;
        BinaryIndexedTree bit = new BinaryIndexedTree(idx, false, null);
        for (int i = 0; i < n; i++) {
            if (preSum[i] >= lower && preSum[i] <= upper) res++;
            // 查询左右边界
            Map.Entry<Long, Integer> left = index.ceilingEntry(preSum[i] - upper), right = index.floorEntry(preSum[i] - lower);
            // 存在则查询
            if (left != null && right != null) {
                res += bit.rangeSum(left.getValue(), right.getValue());
            }
            // 更新计数
            bit.update(index.get(preSum[i]), 1);
        }
        return res;
    }
}

class BinaryIndexedTree {
    int[] bitArr;

    BinaryIndexedTree(int size, boolean init, int[] arr) {
        this.bitArr = new int[size + 1];
        if (init) {
            for (int i = 0; i < size; i++) bitArr[i + 1] = arr[i];
            for (int i = 1; i <= size; i++) {
                int j = i + lowBit(i);
                if (j <= size) bitArr[j] += bitArr[i];
            }
        }
    }

    private int lowBit(int num) {
        return num & -num;
    }

    public void update(int idx, int val) {
        idx++;
        while (idx < bitArr.length) {
            bitArr[idx] += val;
            idx += lowBit(idx);
        }
    }

    private int prefixSum(int idx) {
        idx++;
        int res = 0;
        while (idx > 0) {
            res += bitArr[idx];
            idx -= lowBit(idx);
        }
        return res;
    }

    public int rangeSum(int from, int to) {
        return prefixSum(to) - prefixSum(from - 1);
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：33 ms, 在所有 Java 提交中击败了49.22%的用户。

内存消耗：39.4 MB, 在所有 Java 提交中击败了5.79%的用户。