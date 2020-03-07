[toc]

Given an unsorted array `nums`, reorder it **in-place** such that $nums[0] \le nums[1] \ge nums[2] \le nums[3]\dots$



## 题目解读

&emsp;给定未排序数组，将其排序后满足$nums[0] \le nums[1] \ge nums[2] \le nums[3]\dots$的规律。

```java
class Solution {
    public void wiggleSort(int[] nums) {

    }
}
```

## 程序设计

* 最基础的思路是采用冒泡排序得到有序数组，然后交换峰值索引与其后继的值。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        if(nums == null || nums.length < 2) {
            return;
        }
        // 冒泡排序
        bubbleSort(nums);
        // 峰值所在索引为奇数，左侧已满足条件，维护右侧条件
        for(int i = 1; i < nums.length - 1; i += 2) {
            int temp = nums[i];
            nums[i] = nums[i + 1];
            nums[i + 1] = temp;
        }
    }

    private void bubbleSort(int[] nums) {
        for(int i = 0; i < nums.length; i++) {
            // 中断标识，无需再排序
            boolean flag = true;
            for(int j = 0; j < nums.length - i - 1; j++) {
                if(nums[j] > nums[j + 1]) {
                    int temp = nums[j + 1];
                    nums[j + 1] = nums[j];
                    nums[j] = temp;
                    flag = false;
                }
            }
            if(flag) {
                break;
            }
        }
    }
}
```

* 使用快速排序优化如下：

```java
class Solution {
    public void wiggleSort(int[] nums) {
        if(nums == null || nums.length < 2) {
            return;
        }
        // 快速排序
        quicklySort(nums, 0, nums.length - 1);
        // 峰值所在索引为奇数，左侧已满足条件，维护右侧条件
        for(int i = 1; i < nums.length - 1; i += 2) {
            int temp = nums[i];
            nums[i] = nums[i + 1];
            nums[i + 1] = temp;
        }
    }

    private void quicklySort(int[] nums, int start, int end) {
        if(start >= end) {
            return;
        }
        // 基准值
        int low = start, high = end;
        int base = nums[low];
        while(low < high) {
            while(low < high && nums[high] >= base) high--;
            if(low < high) nums[low++] = nums[high];
            while(low < high && nums[low] <= base) low++;
            if(low < high) nums[high--] = nums[low];
        }
        // 放入准确位置
        nums[low] = base;
        quicklySort(nums, start, low - 1);
        quicklySort(nums, low + 1, end);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：253ms，在所有java提交中击败了5.39%的用户。

内存消耗：42.3MB，在所有java提交中击败了5.62%的用户。

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了45.64%的用户。

内存消耗：42.6MB，在所有java提交中击败了5.62%的用户。

## 官方解题

&emsp;官方除了上述思路还提供了直接交换的思路。遍历当前的元素，如果索引是偶数则是谷底元素，需要小于等于两侧元素，假设我们遍历后维护好了左侧关系，则只需要判断右侧，如果该谷底元素大于后继值，不满足条件，则交换，交换后满足条件，且之前的结构未改变；如果是奇数说明是峰顶元素，若小于后继，则需要交换，交换后满足要求。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            // 谷底元素大于后继，或峰值元素小于等于后继，则交换（这个遍历顺序保证了遍历后的值都是结构正确的）
            if (((i & 1) == 0) == (nums[i] > nums[i + 1])) {
                swap(nums, i, i + 1);
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.17%的用户。

内存消耗：42.4MB，在所有java提交中击败了5.62%的用户。