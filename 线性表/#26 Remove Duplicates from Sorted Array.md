[toc]

Given a sorted array `nums`, remove the duplicates `in-place` such that each element appear only `once` and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** `in-place` with $O(1)$ extra memory.

Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.



## 题目解读

&emsp;给定有序数组，删除重复值，并返回最后的数组长度。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        
    }
}
```

## 程序设计

* 顺序表的删除涉及删除值后的值的前移，为了减少移动，可以从后往前遍历。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length;
        // 记录重复数
        int count;
        for(int i = nums.length - 1; i >= 0; i--) {
            count = 0;
            // 重复计数，最后i停留在第一个值上
            while(i - 1 >= 0 && nums[i] == nums[i - 1]) {
                count++;
                i--;
            }
            // 更新长度
            len -= count;
            // i的count后的值移动到i后
            for(int j = i + 1; j < len; j++) {
                nums[j] = nums[j + count];
            }
        }
        return len;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N^2)$，空间复杂度$O(1)$。

执行用时：25ms，在所有java提交中击败了9.91%的用户。

内存消耗：41.7MB，在所有java提交中击败了6.68%的用户。

## 官方解题

&emsp;实质上不需要移动数组值，只需要替换到对的位置即可。可以采用双指针，遍历替换。`start`、`end`表示相同数字序列的开始索引和结束索引；如果`end`遍历到不同的数值，则需要拼接在`start`后面，同时`start`重新定向到这个新拼接的数值上，即`start=start+1`。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int start = 0;
        for(int end = 1; end < nums.length; end++) {
            // 不相等，则拼接到start后面
            if(nums[end] != nums[start]) {
                // start更新为新的值
                nums[++start] = nums[end];
            }
            // 相等及其不相等，都遍历下一个
        }
        // 最后start就是数组尾部索引，长度为satrt加一
        return start + 1;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.98%的用户。

内存消耗：40.2MB，在所有java提交中击败了57.93%的用户。