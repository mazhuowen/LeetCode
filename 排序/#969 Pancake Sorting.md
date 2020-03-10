[toc]

Given an array `A`, we can perform a pancake flip: We choose some positive integer $k \le \text{A.length}$, then reverse the order of the first **k** elements of `A`.  We want to perform zero or more pancake flips (doing them one after another in succession) to sort the array `A`.

Return the k-values corresponding to a sequence of pancake flips that sort `A`.  Any valid answer that sorts the array within `10 * A.length` flips will be judged as correct.



**Note:**

* $1 \le \text{A.length} \le 100$
* `A[i]` is a permutation of `[1, 2, ..., A.length]`



## 题目解读

&emsp;通过煎饼排序将数组排序为有序数组。输出煎饼排序的操作次序。

```java
class Solution {
    public List<Integer> pancakeSort(int[] A) {

    }
}
```

## 程序设计

* 最基本的思路是如果当前序列是单调的，则继续遍历，否则翻转之前的数组。又分两种情况，以`2,3,4,1`为例，1比当前递增序列的最小值还小，以4为界翻转得`4,3,2,1`，此时已经是有序的了，继续向后遍历；`1,2,4,3`为例，3在前面递增序列的中间，此时可以以3为界翻转，得到`3,4,2,1`，此时需要从头开始继续遍历，遇到2就是第一种情况，翻转得`4,3,2,1`。分析完增序，反序也是一样的道理。代码如下：

```java
class Solution {
    public List<Integer> pancakeSort(int[] A) {
        List<Integer> res = new LinkedList<>();
        if(A == null || A.length < 1) {
            return res;
        }
        int idx = 1;
        while (idx < A.length - 1) {
            // 递增序列遇到较小值
            if(A[idx] >= A[idx - 1] && A[idx] > A[idx + 1]) {
                // 较小值比序列最小值还小，只需翻转序列
                if(A[idx + 1] <= A[0]) {
                    res.add(idx + 1);
                    reverse(A, 0, idx);
                } 
                // 较小值在序列中间，连带这个值和序列一起翻转，同时从头遍历
                else {
                    res.add(idx + 2);
                    reverse(A, 0, idx + 1);
                    idx = 1;
                }
                continue;
            }
            // 递减序列需要较大值，同理
            if(A[idx] <= A[idx - 1] && A[idx] < A[idx + 1]) {
                if(A[idx + 1] >= A[0]) {
                    res.add(idx + 1);
                    reverse(A, 0, idx);
                } else {
                    res.add(idx + 2);
                    reverse(A, 0, idx + 1);
                    idx = 1;
                }
                continue;
            }
            // 单调序列，继续遍历
            idx++;
        }
        // 若最后序列是递减的，还需要翻转下
        if(A[A.length - 1] < A[0]) {
            reverse(A, 0, A.length - 1);
            res.add(A.length);
        }
        return res;
    }

    private void reverse(int[] A, int start, int end) {
        while(start < end) {
            swap(A, start++, end--);
        }
    }

    private void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}
```

* 该思路总体是对的，但有些测试用例生成太长的操作序列导致内存限制达不到要求，即翻转次数超过了数组长度的10倍。简单的全局的考虑，如果每次都查找到当前最大值，将最大值翻转到最前面，然后再次翻转到末尾，此时最大值在准确的位置；然后找到次小值，翻转到最前面，然后倒数第二位整体翻转，这样次小值也在准确位置上。整体每个数值需要翻转两次，符合要求。
* 整理思路可得，首先找到倒数第$n$大的值，此时数组的后$n - 1$已经是有序的了，将这个值翻转到表首，然后翻转后$n - 1$前的整个数组，是的第$n$大的值在倒数第$n$位上，以此类推只到翻转到位置1结束。
* 可优化过程，如果第$n$大的值本来在倒数$n$位，则不用翻转；如果待翻转值在第一位，也不用翻转。

```java
class Solution {
    public List<Integer> pancakeSort(int[] A) {
        List<Integer> res = new LinkedList<>();
        if(A == null || A.length < 1) {
            return res;
        }
        for (int i = A.length - 1; i > 0; i--) {
            int maxIdx = 0;
            for (int j = 0; j <= i; j++) {
                if(A[j] > A[maxIdx]) maxIdx = j;
            }
            // 已经在准确的位置上，继续下一个数值
            if(maxIdx == i) continue;
            // 如果待翻转元素就是第一位，则不用翻转
            if(maxIdx != 0) {
                reverse(A, 0, maxIdx);
                res.add(maxIdx + 1);
            }
            // 翻转整个未排序子序列，使得当前最大值在最后
            reverse(A, 0, i);
            res.add(i + 1);
        }
        return res;
    }

    private void reverse(int[] A, int start, int end) {
        while(start < end) {
            swap(A, start++, end--);
        }
    }

    private void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。



## 官方解题