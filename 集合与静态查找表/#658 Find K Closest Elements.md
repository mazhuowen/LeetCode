[toc]

Given a sorted array, two integers `k` and `x`, find the `k` closest elements to `x` in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.



**Note**:

* The value `k` is positive and will always be smaller than the length of the sorted array.
* Length of the given array is positive and will not exceed $10^4$
* Absolute value of elements in the array and `x` will not exceed $10^4$



## 题目解读

&emsp;查找有序数组中距`x`最近的`k`个数。

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        
    }
}
```

## 程序设计

* 首先使用二分查找定位到第一个大于等于`x`的数，然后判断得到距离`k`最近的位置，通过中心扩展得到`k`个数。


```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        if (k <= 0) throw new IllegalArgumentException("invalid param");
        LinkedList<Integer> res = new LinkedList<>();
        // k大于数组长度，全部装入
        if (k >= arr.length) {
            for (int num : arr) res.addLast(num);
            return res;
        }

        // 二分查找
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] >= x) right = mid;
            else left = mid + 1;
        }
        // 定位最接近的数，left为绝对值最小的索引
        if (left > 0 && arr[left] > x) {
            if (Math.abs(arr[left - 1] - x) < Math.abs(arr[left] - x)) {
                left--;
            }
        }
        // 以left为中心扩展
        int i = left - 1, j = left;
        while ((i >= 0 || j < arr.length) && j - i <= k) {
            // 1、已经扩展到最右边只能扩展左边
            // 2、左边小于等于右边的绝对值
            if (j >= arr.length || i >= 0 && Math.abs(arr[i] - x) <= Math.abs(arr[j] - x)) {
                res.addFirst(arr[i--]);
            } 
            // 已经扩展到最左边或左边值更接近
            else {
                res.addLast(arr[j++]);
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(\log_2N,K)$，空间复杂度为$O(K)$。

执行用时：8ms，在所有java提交中击败了49.48%的用户。

内存消耗：42.3MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;同上。社区还有思路不是查最接近的值，而是查`k`个数的潜在起点终点，通过对比起点终点的优劣来缩小范围。

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {

        List<Integer> res = new LinkedList<>();
        // left、right为k个元素序列的起始索引范围
        int left = 0, right = arr.length - k;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // mid为k-1个序列的前一个，mid+k为k-1个序列的后一个
            // 类似队列出队入队操作，如果mid比mid+k更优，前移
            if (x - arr[mid] <= arr[mid + k] - x) {
                right = mid;
            }
            // mid+k比mid更优，后移
            else {
                left = mid + 1;
            }
        }
        for (int i = 0; i < k; i++) {
            res.add(arr[i + left]);
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(\max(\log_2N,K)$，空间复杂度为$O(K)$。

执行用时：4ms，在所有java提交中击败了99.08%的用户。

内存消耗：41.5MB，在所有java提交中击败了11.11%的用户。