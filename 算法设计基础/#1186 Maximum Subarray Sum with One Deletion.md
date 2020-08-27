[toc]

Given an array of integers, return the maximum sum for a **non-empty** subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.

Note that the subarray needs to be **non-empty** after deleting one element.



**Constraints**:

* $1 \le \text{arr.length} \le 10^5$
* $-10^4 \le \text{arr[i]} \le 10^4$



## 题目解读

&emsp;返回最多删除一个元素后的最大的子数组和。

```java
class Solution {
    public int maximumSum(int[] arr) {
        
    }
}
```

## 程序设计

* 最基本的，使用动态规划数组`f`表示不删除元素的最大值，`g`表示删除一个元素的最大值，则：

```java
class Solution {
    public int maximumSum(int[] arr) {
        int len = arr.length;
        int[] f = new int[len], g = new int[len];
        int res = arr[0]; 
        f[0] = arr[0];
        g[0] = -200001;
        for(int i = 1; i < len; i++){
            f[i] = Math.max(f[i - 1] + arr[i], arr[i]);
            // 不删除当前值与删除当前值比较选择最大值
            g[i] = Math.max(g[i - 1] + arr[i], f[i - 1]);
            res = Math.max(res, Math.max(f[i], g[i]));
        }
        return res;
    }
}
```

* 社区还有使用拼接思路的方法。最多删除一个使得值最大，即当前值为负数，选择左右非负区间；可逆向遍历统计非负区间，然后正向遍历，同时判断合并左右区间。

```java
class Solution {
    public int maximumSum(int[] arr) {
        // 从右往左记录非负数截止i的区间和
    	int right[] = new int[arr.length];
        right[arr.length - 1] = arr[arr.length - 1];
        for (int i = arr.length - 2; i >= 0; i--) {
            right[i] = right[i + 1] < 0 ? arr[i] : right[i + 1] + arr[i];
        }

        // 从左往右记录非负数截止i的区间和
        int left = 0, result = arr[0];
        for (int i = 0; i < arr.length - 1; i++) {
			// 左右都小于0，则尝试当前数字本身
            if (right[i + 1] <= 0 && left <= 0) {
                result = Math.max(arr[i], result);
            } else {
                // 选择左、右、当前大于0的数
                result = Math.max((right[i + 1] > 0 ? right[i + 1] : 0)
                    + (left > 0 ? left : 0)
                    + (arr[i] > 0 ? arr[i] : 0), result);
            }
			// 更新非负数截止左边的区间和
            left = arr[i] + (left <= 0 ? 0 : left);
        }

        result = Math.max(arr[arr.length - 1], result);
        return result;
    }
}
```

## 性能分析

&emsp;动态规划时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：7ms，在所有java提交中击败了62.84%的用户。

内存消耗：48MB，在所有java提交中击败了40.30%的用户。

&emsp;拼接法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了100.00%的用户。

内存消耗：47.6MB，在所有java提交中击败了86.57%的用户。

## 官方解题

&emsp;暂无，密切关注。