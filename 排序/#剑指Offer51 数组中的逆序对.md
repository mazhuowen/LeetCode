[toc]

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

 

**示例 1**:

```
输入: [7,5,6,4]
输出: 5
```



**限制**：

* $0 \le \text{数组长度} \le 50000$



## 题目解读

&emsp;求逆序对。

```java
class Solution {
    public int reversePairs(int[] nums) {

    }
}
```

## 程序设计

* 归并排序。

```java
class Solution {
    public int reversePairs(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        return mergeSort(nums, 0, nums.length - 1);
    }

    private int mergeSort(int[] nums, int left, int right) {
        // 递归终止条件
        if (left >= right) return 0;

        int res = 0;
        // 切分
        int mid = left + (right - left) / 2;
        res += mergeSort(nums, left, mid);
        res += mergeSort(nums, mid + 1, right);
        if (nums[mid] <= nums[mid + 1]) return res;

        // 合并
        int idx = 0;
        int[] tmp = new int[right - left + 1];
        int idx1 = left, idx2 = mid + 1;
        while (idx1 <= mid && idx2 <= right) {
            
            if (nums[idx1] <= nums[idx2]) {
                res += idx2 - mid - 1;
                tmp[idx++] = nums[idx1++];
            } else tmp[idx++] = nums[idx2++];
        }
        while (idx1 <= mid) {
            res += right - mid;
            tmp[idx++] = nums[idx1++];
        }
        while (idx2 <= right) tmp[idx++] = nums[idx2++];
        
        // 复制
        for (int i = 0; i < tmp.length; i++) {
            nums[left + i] = tmp[i];
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：35 ms, 在所有 Java 提交中击败了88.39%的用户

内存消耗：46.4 MB, 在所有 Java 提交中击败了98.64%的用户。

## 官方解题

&emsp;官方除了上述思路，还提供了离散化树状数组的思路。

```java
class Solution {
    public int reversePairs(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        // 离散化
        int[] tmp = new int[nums.length];
        System.arraycopy(nums, 0, tmp, 0, nums.length);
        Arrays.sort(tmp);
        for (int i = 0; i < nums.length; i++) {
            nums[i] = binarySearch(nums[i], tmp) + 1;
        }
        // 树状数组
        int res = 0;
        BinaryIndexedTree bit = new BinaryIndexedTree(nums.length, false, null);
        for (int i = nums.length - 1; i >= 0; i--) {
            res += bit.prefixSum(nums[i] - 1);
            bit.update(nums[i], 1);
        }
        return res;
    }

    private int binarySearch(int target, int[] arr) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] >= target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}

class BinaryIndexedTree {
    int[] bit;

    BinaryIndexedTree(int n, boolean init, int[] arr) {
        this.bit = new int[n + 1];

        if (init) {
            for (int i = 0; i < n; i++) {
                bit[i + 1] = arr[i];
            }
            for (int i = 1; i <= n; i++) {
                int j = i + lowBit(i);
                if (j < bit.length) bit[j] += bit[i];
            }
        }
    }

    private int lowBit(int x) {
        return x & -x;
    }

    public void update(int idx, int val) {
        idx++;
        while (idx < bit.length) {
            bit[idx] += val;
            idx += lowBit(idx);
        }
    }

    public int prefixSum(int idx) {
        idx++;
        int res = 0;
        while (idx > 0) {
            res += bit[idx];
            idx -= lowBit(idx);
        }
        return res;
    }

    public int rangeSum(int from, int to) {
        return prefixSum(from) - prefixSum(to - 1);
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：107 ms, 在所有 Java 提交中击败了5.04%的用户。

内存消耗：47.4 MB, 在所有 Java 提交中击败了83.79%的用户。