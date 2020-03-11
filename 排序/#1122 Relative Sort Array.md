[toc]

Given two arrays `arr1` and `arr2`, the elements of `arr2` are distinct, and all elements in `arr2` are also in `arr1`.

Sort the elements of `arr1` such that the relative ordering of items in `arr1` are the same as in `arr2`.  Elements that don't appear in `arr2` should be placed at the end of `arr1` in **ascending** order.



Constraints:

* $\text{arr1.length, arr2.length} \le 1000$
* $0 \le \text{arr1[i], arr2[i]} \le 1000$
* Each `arr2[i]` is distinct.
* Each `arr2[i]` is in `arr1`.



## 题目解读

&emsp;将数组1根据数组2中的顺序排序。

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {

    }
}
```

## 程序设计

* 首先想到的是统计计数，然后根据数组2的顺序分配位置。

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        if(arr2 == null || arr2.length == 0) {
            return arr2;
        }
        // 统计数字
        int[] counter = new int[1001];
        for (int num : arr1) {
            counter[num]++;
        }
        // 根据数组2排序
        int idx = 0;
        for (int num : arr2) {
            while (counter[num]-- > 0) {
                arr1[idx++] = num;
            }
        }
        // 排序剩余不在数组2中的值
        for (int i = 0; i < counter.length; i++) {
            while (counter[i]-- > 0) {
                arr1[idx++] = i;
            }
        }
        return arr1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.5MB，在所有java提交中击败了5.11%的用户。

## 官方解题

&emsp;暂无，密切关注。