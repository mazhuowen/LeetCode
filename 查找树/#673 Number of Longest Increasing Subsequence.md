[toc]

Given an unsorted array of integers, find the number of longest increasing subsequence.

Note: Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.



## 题目解读

&emsp;给定未排序的数组，找到最长递增子序列的个数。

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {

    }

```

## 程序设计

* 可使用动态规划保存每个位置的最大序列长度和最大序列长度对应的数目。

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        if(nums.length == 0) {
            return 0;
        }
        // 动态规划分别当前位置的递增序列最大长度
        int[] length = new int[nums.length];
        // 动态规划记录当前数字最长序列的个数
        int[] counter = new int[nums.length];
        // 记录最大长度
        int maxLen = 1;
        length[0] = 1; counter[0] = 1;
        for(int i = 1; i < nums.length; i++) {
            length[i] = 1; counter[i] = 1;
            for(int j = i - 1; j >= 0; j--) {
                // 当前元素大于递增序列末尾，且更新后序列大于当前序列
                if(nums[i] > nums[j] && length[j] + 1 > length[i]) {
                    length[i] = length[j] + 1;
                    counter[i] = counter[j];
                } 
                // 叠加
                else if(nums[i] > nums[j] && length[j] + 1 == length[i]) {
                    counter[i] += counter[j];
                }
            }
            maxLen = Math.max(maxLen, length[i]);
        }
        int count = 0;
        for(int i = 0; i < counter.length; i++) {
            if(length[i] == maxLen) {
                count += counter[i];
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;动态规划时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：18ms，在所有java提交中击败了33.70%的用户。

内存消耗：39.4MB，在所有java提交中击败了10.53%的用户。

## 官方解题

&emsp;除了基本的动态规划，官方介绍了线段树的思路。首先定义动态线段树，需要注意区间是数组中的数值范围，不是索引。结点还有最大序列长度和计数，表示序列结尾是在这个数值范围内的所有数的最大值。可见叶结点保存序列结尾为节点值的序列长度。根据题目要求，需要设计查询，支持查询序列结尾不大于查询数的最大序列长度。其次需要支持更新操作，更新涉及到父节点的数值更新。

```java
class Tree {
    // 区间的最小数值和最大数值
    int start, end;
    Tree left, right;
    // 序列尾部为区间中的数的最大序列长度和数值
    int maxLen = 0;
    int count = 1;

    Tree(int start, int end) {
        this.start  = start;
        this.end = end;
    }
    // 返回区间范围中间值
    private int getMid() {
        return start + (end - start) / 2;
    }

    private Tree left() {
        if(left == null) {
            left = new Tree(start, getMid());
        }
        return left;
    }

    private Tree right() {
        if(right == null) {
            right = new Tree(getMid() + 1, end);
        }
        return right;
    }
    // 合并（序列长度一致，则计数相加，不一致选择长的序列）
    private int[] merge(int[] val1, int[] val2) {
        if(val1[0] == val2[0]) {
            if(val1[0] == 0) {
                return val1;
            }
            val1[1] += val2[1];
            return val1;
        } else {
            return val1[0] > val2[0] ? val1 : val2;
        }
    }

    // 查询小于high的区间的最大序列长度和数目
    public int[] query(int high) {
        // 当前范围小于high，返回值
        if(end <= high) {
            return new int[]{maxLen, count};
        }
        // 不在当前范围
        if(high < start) {
            return new int[]{0, 1};
        }
        // 在当前范围，递归查找
        int[] leftVal = left().query(high);
        int[] rightVal = right().query(high);
        // 合并子树结果
        return merge(leftVal, rightVal);
    }

    // 更新小于等于区间high的序列长度和计数
    public void update(int high, int len, int c) {
        // 递归终止条件（即high结点对应的最长序列）
        if(start == end) {
            int[] cur = merge(new int[]{maxLen, count}, new int[]{len, c});
            maxLen = cur[0];
            count = cur[1];
            return;
        } 
        // 区间在左子树
        if(high <= getMid()) {
            left().update(high, len, c);
        } else {
            right().update(high, len, c);
        }
        // 子树更新完，更新父节点
        int[] cur = merge(new int[]{left().maxLen, left().count}, new int[]{right().maxLen, right().count});
        maxLen = cur[0];
        count = cur[1];
    }
}
```

在设计好线段树后，功能实现如下：

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        if(nums.length == 0) {
            return 0;
        }
        // 找到数值区间
        int min = nums[0], max = nums[0];
        for(int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }
        Tree tree = new Tree(min, max);
        for(int num : nums) {
            // 查询小于num的最大序列和计数
            int[] res = tree.query(num - 1);
            // 更新num的序列（在原先序列加一，计数不变）
            tree.update(num, res[0] + 1, res[1]);
        }
        return tree.count;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：10ms，在所有java提交中击败了92.29%的用户。

内存消耗：42.2MB，在所有java提交中击败了5.26%的用户。