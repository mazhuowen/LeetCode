[toc]

Given an array `A` of non-negative integers, return an array consisting of all the even elements of `A`, followed by all the odd elements of `A`.

You may return any answer array that satisfies this condition.



**Note:**

* $1 \le \text{A.length} \le 5000$
* $0 \le \text{A[i]} \le 5000$



## 题目解读

&emsp;将数组中偶数放到奇数前面。

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {

    }
}
```

## 程序设计

* 类似于快速排序，双指针遍历交换。

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        if (A == null || A.length == 0) return A;
        int even = 0, odd = A.length - 1;
        while (even < odd) {
            while (even < odd && A[odd] % 2 == 1) odd--;
            while (even < odd && A[even] % 2 == 0) even++;

            if (even < odd) swap(A, even, odd);
        }
        return A;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.5MB，在所有java提交中击败了71.43%的用户。

## 官方解题