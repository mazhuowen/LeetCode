[toc]

Given an unsorted array, find the maximum difference between the successive elements in its sorted form.

Return 0 if the array contains less than 2 elements.



**Note**:

* You may assume all elements in the array are non-negative integers and fit in the 32-bit signed integer range.
* Try to solve it in linear time/space.



## 题目解读

&emsp;给定一个无序的数组，找出数组在排序之后，相邻元素之间最大的差值。

```java
class Solution {
    public int maximumGap(int[] nums) {

    }
}
```

## 程序设计

* 最简单的思路是先用快速排序，然后两两遍历计算结果。

```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums == null || nums.length < 2) {
            return 0;
        }
        Arrays.sort(nums);
        int max = 0;
        for(int i = 0; i < nums.length - 1; i++) {
            max = Math.max(max, nums[i + 1] - nums[i]);
        }
        return max;
    }
}
```

* 根据题目要求，时间复杂度需要线性，首先想到的是基数排序，实现如下：

```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums == null || nums.length < 2) {
            return 0;
        }
        nums = radixSort(nums);
        int max = 0;
        for(int i = 0; i < nums.length - 1; i++) {
            max = Math.max(max, nums[i + 1] - nums[i]);
        }
        return max;
    }

    private int[] radixSort(int[] nums) {
        // 10个口袋
        Node[] bucket = new Node[10];
        Node[] last = new Node[10];
        // 记录链表头尾
        Node head = null, tail = null;
        // 统计最大值并初始化链表
        int max = 0;
        for(int num : nums) {
            max = Math.max(max, num);
            if(head == null) {
                head = tail = new Node(num);
            } else {
                tail.next = new Node(num);
                tail = tail.next;
            }
        }
        
        // 基数
        int base = 1;
        // 分配d位
        while(max / base > 0) {
            // 分配
            while(head != null) {
                int idx = head.val / base % 10;
                if(bucket[idx] == null) {
                    bucket[idx] = head;
                    last[idx] = head;
                } else {
                    last[idx].next = last[idx] = head;
                }
                head = head.next;
            }
            // 重组
            for(int i = 9; i >=0; i--) {
                if(bucket[i] == null) continue;
                if(head == null) {
                    head = bucket[i];
                } else {
                    tail.next = bucket[i];
                }
                tail = last[i];
                // 清空
                bucket[i] = last[i] = null;
            }
            // 断开
            tail.next = null;
            // 迭代
            base *= 10;
        }
		// 重组有序数组
        int[] newNums = new int[nums.length];
        for(int i = nums.length - 1; i >= 0; i--) {
            newNums[i] = head.val;
            head = head.next;
        }
        return newNums;
    }
}
// 链表结构
class Node {
    int val;
    Node next;

    Node(int val) {
        this.val = val;
    }
}
```

## 性能分析

&emsp;快速排序时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.1MB，在所有java提交中击败了5.26%的用户。

&emsp;基数排序时间复杂度为$O(N + K)$，由于此处$K=10$，故时间复杂度为$O(N)$；空间复杂度为$O(N + K)$，即$O(N)$。

执行用时：4ms，在所有java提交中击败了74.59%的用户。

内存消耗：39.6MB，在所有java提交中击败了5.26%的用户。

## 官方解题

&emsp;官方提供的基数排序融合了计数排序，从而不使用链表实现：

```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums == null || nums.length < 2) {
            return 0;
        }
        radixSort(nums);
        int max = 0;
        for(int i = 0; i < nums.length - 1; i++) {
            max = Math.max(max, nums[i + 1] - nums[i]);
        }
        return max;
    }
    // 基数排序
    private void radixSort(int[] nums) {
        // 统计最大数
        int max = 0;
        for(int num : nums) {
            max = Math.max(max, num);
        }
        int base = 1;
        // 分配
        while(max / base > 0) {
            countSort(nums, base);
            base *= 10;
        }
    }
    // 计数排序（基于基数排序的改进版）
    private void countSort(int[] nums, int base) {
        int[] counter = new int[10];
        for(int num : nums) {
            // 根据base位的数排序（1为个位数，10为十位数等）
            counter[num / base % 10] ++;
        }
        // 统计结果前缀和相加
        for(int i = 1; i <= 9; i++) {
            counter[i] += counter[i - 1];
        }
        // 新建排序表
        int[] temp = new int[nums.length];
        for(int i = nums.length - 1; i >= 0; i--) {
            int idx = nums[i] / base % 10;
            temp[--counter[idx]] = nums[i];
        }
        // 复制到nums
        for(int i = 0; i < nums.length; i++) {
            nums[i] = temp[i];
        }
    }
}
```

时间、空间复杂度不变。

&emsp;除了上述思路，官方还提供了最优的桶的思路。元素之间的间距考虑最好情况，假设元素排好序且两两之间间距相同。这意味着任意相邻元素都有恒定的差值。所以$n$个元素有$n - 1$个间距，假设为$t$，显然可以得到$t = (\text{max} - \text{min})/(n-1)$，其中$\max$和$\min$是数组中最大和最小的元素。这个间距就是相邻元素间最大间距。现在考虑间距不相等的情况，假设其它元素间距仍然是$t$，元素$i-1$和$i$之间把间距变为$t-p$，则相应的$i$和$i+1$之间间距变为$t + p$，从而最大间距是$t+p$。

&emsp;如果将上述思想应用到相邻的等距离区间，每个区间都是相邻的开闭区间，如`6,9,21,102`有三个区间`[6,9)`，`[9,21)`，`[21,102)`，但是不等距，可以将这三个区间等距化，变为`[6,38)`，`[38,70)`，`[70,102)`，将原先的元素分配到这些新的区间，并记录每个区间的最小和最大值。未平均前每个区间都有同一个值（多个或一个），平均后有的区间有多个不重复值或没有。根据之前的分析，这些距离减少，必然有距离增大，故最大距离肯定不是桶内非边界值的值。这样我们只需遍历计算桶之间的距离和桶最大最小值的距离即可。

```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums == null || nums.length < 2) {
            return 0;
        }
        // 统计最大最小值
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for(int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        // 在 n 个数下，形成的两两相邻区间是 n - 1 个，比如 [2,4,6,8] 这里有 4 个数，但是只有 3 个区间，[2,4], [4,6], [6,8]
        // 因此，桶长度 = 区间总长度 / 区间总个数 = (max - min) / (nums.length - 1)
        int bucketSize = Math.max(1, (max - min) / (nums.length - 1));
        // 桶个数 = 区间长度 / 桶长度 加一，因为整数除法会向下取整
        int k = (max - min) / bucketSize + 1;
        Bucket[] bucket = new Bucket[k];
        // 初始化桶
        for(int i = 0; i < nums.length; i++) {
            int idx = (nums[i] - min) / bucketSize;
            if(bucket[idx] == null) bucket[idx] = new Bucket();
            
            bucket[idx].min = Math.min(bucket[idx].min, nums[i]);
            bucket[idx].max = Math.max(bucket[idx].max, nums[i]);
        }
        int maxGap = Integer.MIN_VALUE, preBucket = -1;
        // 遍历桶计算
        for(int i = 0; i < k; i++) {
            if(bucket[i] == null) continue;
            // 计算桶间距离
            if(preBucket != -1) {
                maxGap = Math.max(maxGap, bucket[i].min - preBucket);
            }
            // 更新桶内间距（由上面分析可知无需检查计算桶内其它结点距离）
            maxGap = Math.max(maxGap, bucket[i].max - bucket[i].min);
            preBucket = bucket[i].max;
        }
        return maxGap;
    }
}

class Bucket {
    int min = Integer.MAX_VALUE;
    int max = Integer.MIN_VALUE;
}
```

&emsp;时间复杂度为$O(N + K)$，$K$为桶的个数；空间复杂度为$O(K)$。

执行用时：4ms，在所有java提交中击败了74.59%的用户。

内存消耗：39.1MB，在所有java提交中击败了5.26%的用户。