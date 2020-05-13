[toc]

Given a positive **32-bit** integer **n**, you need to find the smallest **32-bit** integer which has exactly the same digits existing in the integer **n** and is greater in value than **n**. If no such positive **32-bit** integer exists, you need to return $-1$.



## 题目解读

&emsp;给定整数，获得大于这个整数且由该整数数位构成的最小值。

```java
class Solution {
    public int nextGreaterElement(int n) {

    }
}
```

## 程序设计

* 当整数从高位到低位是逆序时，如`654321`时最大，当整数为顺序时，如`123456`时最小。大于给定数字的最小整数是从低位遍历，遇到不满足逆序的第一个数字，替换为其后大于该数字的最小值，然后排序其后的数字使得呈现顺序。如`123543`，从低位开始不满足条件的数字是3，将3替换为其后大于3的最小值4，并排序剩余数字得到`124335`。

```java
class Solution {
    public int nextGreaterElement(int n) {
        if (n < 10) return -1;
        String num = Integer.toString(n);
        char[] nums = num.toCharArray();
        // 查找拐点索引
        int i;
        for (i = nums.length - 1; i >= 1; i--) {
            if (nums[i] > nums[i - 1]) break;
        }
        // 数字是逆序的，即已经是最大数字
        if (i == 0) return -1;
        // 将i-1位置替换为后面大于i-1的最小值
        int idx = binarySearch(nums, i, nums.length - 1, nums[i - 1]);
        swap(nums, i - 1, idx);
        
        // 翻转逆序对为顺序对
        reverse(nums, i, nums.length - 1);

        // 注意数据溢出的情况
        long res = Long.valueOf(new String(nums));
        return res > Integer.MAX_VALUE ? -1 : (int)res;
    }

    private void swap(char[] nums, int i, int j) {
        char temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    private void reverse(char[] nums, int i, int j) {
        while (i < j) {
            swap(nums, i++, j--);
        }
    }

    private int binarySearch(char[] nums, int i, int j, char target) {
        int left = i, right = j;
        while (left < right) {
            int mid = left + (right - left) / 2 + 1;
            if (nums[mid] <= target) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方思路类似。