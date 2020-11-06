[toc]

Given an array with $n$ objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.



**Note**: You are not suppose to use the library's sort function for this problem.

**Follow up**:

* A rather straight forward solution is a two-pass algorithm using counting sort.
  First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
* Could you come up with a one-pass algorithm using only constant space?



## 题目解读

&emsp;一个数组只包含0、1、2，题目要求不使用封装方法排序，特别提示计数排序。

```java
class Solution {
    public void sortColors(int[] nums) {

    }
}
```

## 程序设计

* 题目说的计数排序如下：

```java
class Solution {
    public void sortColors(int[] nums) {
        if(nums == null || nums.length == 0) {
            return;
        }
        // 计数
        int[] counter = new int[3];
        for(int num : nums) {
            counter[num] += 1;
        }
        // 前缀和
        for(int i = 1; i < counter.length; i++) {
            counter[i] += counter[i - 1];
        }
        // 排序
        int[] temp = new int[nums.length];
        for(int i = nums.length - 1; i >= 0; i--) {
            temp[--counter[nums[i]]] = nums[i];
        }
        // 赋值（由于nums是引用对象，赋值temp不改变nums指向的对象，必须一个个赋值）
        for(int i = 0; i < nums.length; i++) {
            nums[i] = temp[i];
        }
    }
}
```

## 性能分析

&emsp;计数排序时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.9MB，在所有java提交中击败了5.17%的用户。

## 官方解题

&emsp;官方提出空间复杂度为常量的解法，即荷兰国旗问题。通过三个指针的迭代交换完成排序。

```java
class Solution {
    public void sortColors(int[] nums) {
        if(nums == null || nums.length == 0) {
            return;
        }
        // first指向目前已知0的最右边，second指向已知2的最左端，cur在中间遍历
        int first = 0, second = nums.length - 1, cur = 0;
        while(cur <= second) {
            // 红色需要插入到边界first
            if (nums[cur] == 0) {
                int temp = nums[first];
                nums[first++] = nums[cur];
                // 由于cur之前first之后的元素必然是1（如果存在），交换后不破坏有序性，则没必要继续判断cur，继续迭代
                // 如果不存1，此时cur交换后为2，继续遍历，因为遇到0后会和刚才的2继续交换
                nums[cur++] = temp;
            } 
            // cur为2需要插入到second
            else if (nums[cur] == 2) {
                int temp = nums[second];
                nums[second--] = nums[cur];
                // 由于交换到cur的还是2，需要继续判断
                nums[cur] = temp;
            } 
            // 为1，继续遍历
            else cur++;
        }
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB，在所有java提交中击败了5.17%的用户。