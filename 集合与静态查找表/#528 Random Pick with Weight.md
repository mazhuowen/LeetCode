[toc]

Given an array `w` of positive integers, where `w[i]` describes the weight of index `i`, write a function `pickIndex` which randomly picks an index in proportion to its weight.

Note:

* $1 \le \text{w.length} \le 10000$
* $1 \le \text{w[i]} \le 10^5$
* `pickIndex` will be called at most `10000` times.



## 题目解读

&emsp;根据权重随机选择数据。

```java
class Solution {

    public Solution(int[] w) {

    }
    
    public int pickIndex() {

    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(w);
 * int param_1 = obj.pickIndex();
 */
```

## 程序设计

* 将权重用前缀和表示，然后使用随机数进行二分查找即可。

```java
class Solution {
    Random random;
    int[] preSum;
    int limit;

    public Solution(int[] w) {
        this.random = new Random();
        this.preSum = new int[w.length + 1];
        // 计算前缀和
        for (int i = 0; i < w.length; i++) {
            preSum[i + 1] = preSum[i] + w[i];
        }
        this.limit = preSum[preSum.length - 1];
    }
    
    public int pickIndex() {
        // 避免产生0，生成数据范围在1～limit
        int val = random.nextInt(limit) + 1;
        return binarySearch(preSum, val);
    }

    private int binarySearch(int[] nums, int val) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= val) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        // 由于是前缀和，索引减一
        return left - 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2W)$，空间复杂度为$O(N)$。

执行用时：33ms，在所有java提交中击败了86.38%的用户。

内存消耗：44.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。