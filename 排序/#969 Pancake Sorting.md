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

* 该思路总体是对的，但有些测试用例生成太长的操作序列导致内存限制达不到要求。



## 性能分析



## 官方解题