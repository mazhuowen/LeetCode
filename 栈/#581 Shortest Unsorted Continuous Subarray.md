[toc]

Given an integer array, you need to find one **continuous subarray** that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the **shortest** such subarray and output its length.



Note:

* Then length of the input array is in range `[1, 10,000]`.
* The input array may contain duplicates, so ascending order here means <=.



## 题目解读

&emsp;给定一个整数数组，寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。输出最短的子数组长度。

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {

    }
}
```

## 程序设计

* 最基本的思路是排序遍历两侧不一致的索引，从而得到长度。

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] temp = Arrays.copyOf(nums, nums.length);
        Arrays.sort(temp);
        int left = 0, right = nums.length - 1;
        while (left <= right && nums[left] == temp[left]) left++;
        while (left <= right && nums[right] == temp[right]) right--;
        if (left > right) return 0;
        return right - left + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了63.65%的用户。

内存消耗：40.7MB，在所有java提交中击败了19.05%的用户。

## 官方解题

&emsp;除了上述最基本的思路，官方还提供了单调栈的思路：

```java
public class Solution {
    public int findUnsortedSubarray(int[] nums) {
        Stack < Integer > stack = new Stack < Integer > ();
        int left = nums.length, right = 0;
        // 单调递增栈
        for (int i = 0; i < nums.length; i++) {
            // 当前值比前面的值小，即应该交换到前面
            while (!stack.isEmpty() && nums[stack.peek()] > nums[i])
                // 记录最前面的索引
                left = Math.min(left, stack.pop());

            stack.push(i);
        }
        // 单调递减栈
        stack.clear();
        for (int i = nums.length - 1; i >= 0; i--) {
            // 记录比后面的值大的索引，应该交换到后面
            while (!stack.isEmpty() && nums[stack.peek()] < nums[i])
                // 记录最大的索引
                right = Math.max(right, stack.pop());

            stack.push(i);
        }
        // 左右索引之间的区段需要排序才能维持整体升序
        return right - left > 0 ? right - left + 1 : 0;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：19ms，在所有java提交中击败了17.69%的用户。

内存消耗：40.4MB，在所有java提交中击败了23.81%的用户。

&emsp;上述思路可见最小值和最大值应该所在的位置决定了区间长度，可多次遍历查找。

```java
public class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        boolean flag = false;
        // 遍历查找不满足升序的最小值最大值
        for (int i = 1; i < nums.length; i++) {
            // 区间不满足升序
            if (nums[i] < nums[i - 1]) flag = true;
            // 统计最小的不满足升序的数值
            if (flag) min = Math.min(min, nums[i]);
        }
        // 同理统计最大值
        flag = false;
        for (int i = nums.length - 2; i >= 0; i--) {
            if (nums[i] > nums[i + 1])
                flag = true;
            if (flag)
                max = Math.max(max, nums[i]);
        }
        // 第二次遍历确定最小值、最大值应该在的位置
        int l, r;
        for (l = 0; l < nums.length; l++) {
            if (min < nums[l])
                break;
        }
        for (r = nums.length - 1; r >= 0; r--) {
            if (max > nums[r])
                break;
        }
        return r - l < 0 ? 0 : r - l + 1;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了76.69%的用户。

内存消耗：40.8MB，在所有java提交中击败了19.05%的用户。