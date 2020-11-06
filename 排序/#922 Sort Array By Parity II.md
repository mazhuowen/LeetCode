[toc]

Given an array A of non-negative integers, half of the integers in A are odd, and half of the integers are even.

Sort the array so that whenever `A[i]` is odd, `i` is odd; and whenever `A[i]` is even, `i` is even.

You may return any answer array that satisfies this condition.



**Note:**

* $2 \le \text{A.length} \le 20000$
* $\text{A.length} \% 2 == 0$
* $0 \le \text{A[i]} \le 1000$



## 题目解读

&emsp;给定数组偶数长度，数组包含一半偶数值，一半奇数值，排序使得偶数值在偶数索引上，奇数值在奇数索引上。

```java
class Solution {
    public int[] sortArrayByParityII(int[] A) {

    }
}
```

## 程序设计

* 首先由于必然存在一半偶数一半奇数，故只需维护好偶数索引的准确性即可，此时奇数索引必然是奇数。

```java
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        if(A ==null || A.length == 0) {
            return A;
        }
        for(int i = 0; i < A.length - 1; i += 2) {
            // 满足条件
            if(A[i] % 2 == 0 && A[i + 1] % 2 == 1) {
                continue;
            }
            // 奇数偶数互换
            if(A[i] % 2 == 1 && A[i + 1] % 2 == 0) {
                swap(A, i, i + 1);
            }
            // 都是偶数，奇数索引上是偶数，错误
            else if(A[i] % 2 == 0) {
                for(int j = i + 2; j < A.length; j += 2) {
                    if(A[j] % 2 == 1) swap(A, i + 1, j);
                }
            }
            // 都是奇数，偶数索引上是奇数，错误
            else {
                for(int j = i + 3; j < A.length; j += 2) {
                    if(A[j] % 2 == 0) swap(A, i, j);
                }
            }
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

* 上述思路每次都要重复遍历不对的元素后面的值，后面可能大多是正确的，造成重复遍历。可以使用双指针记录奇、偶索引。

```java
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        int j = 1;
        // 遍历偶索引
        for (int i = 0; i < A.length; i += 2)
            // 偶索引数值不是偶数，则必然有错误值在奇数索引
            if (A[i] % 2 == 1) {
                // 遍历奇数索引，直到遇到不正确的值
                while (A[j] % 2 == 1)
                    j += 2;

                // 交换即可
                int tmp = A[i];
                A[i] = A[j];
                A[j] = tmp;
            }

        return A;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了99.94%的用户。

内存消耗：43.1MB，在所有java提交中击败了5.08%的用户。

## 官方解题

&emsp;同上。