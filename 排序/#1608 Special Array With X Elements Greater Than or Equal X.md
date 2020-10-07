[toc]

You are given an array nums of non-negative integers. `nums` is considered **special** if there exists a number `x` such that there are **exactly** `x` numbers in `nums` that are **greater than or equal** to `x`.

Notice that `x` **does not** have to be an element in `nums`.

Return `x` if the array is **special**, otherwise, return $-1$. It can be proven that if `nums` is special, the value for `x` is **unique**.



**Constraints:**

- $1 \le \text{nums.length} \le 100$
- $0 \le \text{nums[i]} \le 1000$



## 题目解读

&emsp;如果存在一个数`x`，使得`nums`中恰好有`x`个元素大于或者等于`x`，那么就称`nums`是一个特殊数组，而`x`是该数组的特征值。求该值。

```java
class Solution {
    public int specialArray(int[] nums) {

    }
}
```

## 程序设计

* 基本的思路是排序然后对比当前数字和剩余长度，此处需注意`0,3,6,6,7`当遍历到索引$2$时，此时$6$大于剩余长度$3$，但是`x=-1`，因为$6$前的$3$也大于等于$3$，故比较时还需要比较前面的数字是否小于当前`x`。

```java
class Solution {
    public int specialArray(int[] nums) {
        if (nums == null || nums.length == 0) return -1;
        // 排序
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            int x = nums.length - i;
            // 判断x是否符合要求（如果前面存在大于等于x的数，则返回-1）
            if (nums[i] >= x) return i == 0 || nums[i - 1] < x ? x : -1;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了96.99%的用户。

内存消耗：37MB，在所有java提交中击败了10.63%的用户。

## 官方解题

&emsp;暂无，密切关注。