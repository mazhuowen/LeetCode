[toc]

Given an array nums and a value val, remove all instances of that value `in-place` and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input** array `in-place` with $O(1)$ extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.



## 题目解读

&emsp;给定数组和数值，删除该数值，并返回数组长度。注意题目提示可以变动最终元素的相对位置。

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        
    }
}
```

## 程序设计

* 可参考[#26 Remove Duplicates from Sorted Array](./#26 Remove Duplicates from Sorted Array.md)的思路，`start`表示下一个数值需要插入的位置，`end`则遍历数组，当不是`val`时，拼接到`start`位置，`strat`加$1$；当`end`是`val`值时，继续遍历。

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        // start表示下一个不是val的值在新数组中的索引
        int start = 0;
        for(int end = 0; end < nums.length; end++) {
            // 需要前移
            if(nums[end] != val) {
                nums[start++] = nums[end];
            }
        }
        // 遍历结束start指向size+1，就是数组长度
        return start;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.4MB，在所有java提交中击败了23.33%的用户。

## 官方解题

&emsp;除了上述不改变元素相对位置的思路，官方还提供了改变位置的思路，即把需要删除的值替换掉。

```java
public int removeElement(int[] nums, int val) {
    int i = 0;
    int n = nums.length;
    // 遍历
    while (i < n) {
        // 与队尾替换，注意i不更新，下一轮继续在i遍历
        if (nums[i] == val) {
            nums[i] = nums[n - 1];
            // 下一次尾部替换索引减一
            n--;
        } else {
            // 不是val，继续下次替换
            i++;
        }
    }
    return n;
}
```

当数组中`val`值比较少时，该方法很适合。