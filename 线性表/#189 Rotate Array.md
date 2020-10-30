[toc]

Given an array, rotate the array to the right by k steps, where k is non-negative.



**Note**:

* Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
* Could you do it in-place with O(1) extra space?



## 题目解读

&emsp;将数组右移$k$位。

```java
class Solution {
    public void rotate(int[] nums, int k) {

    }
}
```

## 程序设计

* 最基本的思路是右移一位，总共操作$K$次；还有引入额外空间保存移动后的数字。
* 可以利用循环移动来完成数组的整体移动；但是数组中可能存在多个循环链，需要统计移动结点数目来确定是否要继续移动。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        if (k == 0) return;
        // 左移相当于右移
        if (k < 0) {
            k = (-k) % nums.length;
            rotate(nums, nums.length - k);
            return;
        }
        
        k %= nums.length;

        // 移动的结点数目
        int count = 0;
        // 开始移动过位置
        for (int start = 0; count < nums.length; start++) {
            // 将cur移动到next
            int cur = start, next;
            // cur的值
            int curVal = nums[start];
            do {
                next = (cur + k) % nums.length;
                // 移动并保存next的值到curVal
                int temp = nums[next];
                nums[next] = curVal;
                curVal = temp;
                // 计数减少
                count++;
                // 迭代
                cur = next;
            } while (start != cur);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;官方除了循环移动，还提出了翻转数组的思路，类似煎饼排序。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        if (k == 0) return;
        // 左移相当于右移
        if (k < 0) {
            k = (-k) % nums.length;
            rotate(nums, nums.length - k);
            return;
        }
        
        k %= nums.length;

        // 翻转整个数组
        reverse(nums, 0, nums.length - 1);
        // 翻转前k个
        reverse(nums, 0, k - 1);
        // 翻转剩余的
        reverse(nums, k, nums.length - 1);
    }

    private void reverse(int[] nums, int s, int e) {
        while (s < e) {
            swap(nums, s++, e--);
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] =  nums[j];
        nums[j] = temp;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了66.04%的用户。

内存消耗：40MB，在所有java提交中击败了7.14%的用户。