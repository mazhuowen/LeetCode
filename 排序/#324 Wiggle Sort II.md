[toc]

Given an unsorted array `nums`, reorder it such that `nums[0] < nums[1] > nums[2] < nums[3]....`

Note:
You may assume all input has valid answer.

Follow Up:
Can you do it in $O(n)$ time and/or in-place with $O(1)$ extra space?



## 题目解读

&emsp;摆动排序。

```java
class Solution {
    public void wiggleSort(int[] nums) {

    }
}
```

## 程序设计

* 首先想到的是排序数组，然后将后半部分穿插进前半部分，这样由于后半部分值大于前半部分，穿插后得到的组合符合要求。但是测试用例`[4,5,5,6]`不通过，通过分析可知当前后两部分存在相同元素`val`，此时`val`必然为前半部分的最大值，后半部分的最小值，如果`val`在前后两部分存在多个重复值，比如达到各部分一半，则会出现穿插后向后元素相等的情况。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        if(nums == null || nums.length < 2) {
            return;
        }
        // 排序
        Arrays.sort(nums);
        int[] temp = new int[nums.length];
        int d = (nums.length + 1) / 2;
        for(int i = 0; i < nums.length; i++) {
            if(i < d) {
                temp[2 * i] = nums[i];
            } else {
                temp[(i - d) * 2 + 1 ] = nums[i];
            }
        }
        for(int i = 0; i < nums.length; i++) {
            nums[i] = temp[i];
        }
    }  
}
```

* 为了最大可能避免这种情况，应该尽可能让相同元素分开，但又要保持有序性。可以将前后两部分分别反序后再穿插。此时原先的尾、头变成头尾，相距较远。如果还发生前后元素相同的情况，则必然无法组成摆动排序。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        if(nums == null || nums.length < 2) {
            return;
        }
        Arrays.sort(nums);
        int[] temp = Arrays.copyOf(nums, nums.length);
        // 记录末尾索引，从尾部进行插入，达到反序的目的
        int smaIdx = (nums.length + 1) / 2 - 1;
        int bigIdx = nums.length - 1;
        for(int i = 0; i < nums.length / 2; i++) {
            // 小值
            nums[2 * i] = temp[smaIdx--];
            // 大值
            nums[2 * i + 1] = temp[bigIdx--];
        }
        if(nums.length % 2 == 1) {
            nums[nums.length - 1] = temp[0];
        }
    }
}
```

* 延续上面思路，如果是$O(N)$时间复杂度，则可以使用基数排序进行。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        if(nums == null || nums.length < 2) {
            return;
        }
        Arrays.sort(nums);
        int[] temp = Arrays.copyOf(nums, nums.length);
        int smaIdx = (nums.length + 1) / 2 - 1;
        int bigIdx = nums.length - 1;
        for(int i = 0; i < nums.length / 2; i++) {
            // 小值
            nums[2 * i] = temp[smaIdx--];
            // 大值
            nums[2 * i + 1] = temp[bigIdx--];
        }
        if(nums.length % 2 == 1) {
            nums[nums.length - 1] = temp[0];
        }
    }

    private void radixSort(int[] nums) {
        int max = Integer.MIN_VALUE;
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

    private void countSort(int[] nums, int base) {
        int[] counter = new int[10];
        int[] temp = new int[nums.length];
        for(int num : nums) {
            counter[num / base % 10] += 1;
        }
        for(int i = 1; i < counter.length; i++) {
            counter[i] += counter[i - 1];
        }
        for(int i = nums.length - 1; i >= 0; i--) {
            temp[counter[nums[i] / base % 10] - 1] = nums[i];
        }
        for(int i = 0; i < nums.length; i++) {
            nums[i] = temp[i];
        }
    }
}
```

## 性能分析

&emsp;基于快速排序的时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了95.25%的用户。

内存消耗：44.3MB，在所有java提交中击败了5.07%的用户。

&emsp;基于基数排序的时间复杂度为$O(N)$，空间复杂度为$O(N)$。

## 官方解题

&emsp;暂无，密切关注。