[toc]

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。



**提示**:

* $0 < \text{nums.length} \le 100$

**说明**:

* 输出结果可能非常大，所以你需要返回一个字符串而不是整数
* 拼接起来的数字可能会有前导 $0$，最后结果不需要去掉前导 $0$



## 题目解读

&emsp;返回数字拼接的最小数值。

```java
class Solution {
    public String minNumber(int[] nums) {

    }
}
```

## 程序设计

* 观测发现将数字两两拼接对比，结果小的值所对应的顺序是符合要和的，故可对数组排序，然后拼接。

```java
class Solution {
    public String minNumber(int[] nums) {
        quickSort(nums, 0, nums.length - 1);
        StringBuffer res = new StringBuffer();
        for (int num : nums) res.append(num);
        return res.toString(); 
    }

    // 快速排序
    private void quickSort(int[] nums, int l, int r) {
        if (l >= r) return;
        int base = nums[l];
        int left = l, right = r;
        while (left < right) {
            while (left < right && compare(nums[right], base) >= 0) right--;
            if (left < right) nums[left++] = nums[right];
            while (left < right && compare(nums[left], base) <= 0) left++;
            if (left < right) nums[right--] = nums[left];
        }
        nums[left] = base;

        quickSort(nums, l, left - 1);
        quickSort(nums, left + 1, r);
    }

    private int compare(int num1, int num2) {
        StringBuffer sb1 = new StringBuffer();
        sb1.append(num1).append(num2);
        StringBuffer sb2 = new StringBuffer();
        sb2.append(num2).append(num1);
        return sb1.toString().compareTo(sb2.toString());
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了90.46%的用户。

内存消耗：38.7MB，在所有java提交中击败了20.62%的用户。

## 官方解题

&emsp;暂无，密切关注。