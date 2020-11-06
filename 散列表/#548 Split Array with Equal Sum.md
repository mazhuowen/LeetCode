[toc]

Given an array with n integers, you need to find if there are triplets (i, j, k) which satisfies following conditions:

* $0 < i$, $i + 1 < j$, $j + 1 < k < n - 1$
* Sum of subarrays $(0, i - 1)$, $(i + 1, j - 1)$, $(j + 1, k - 1)$ and $(k + 1, n - 1)$ should be equal.

where we define that subarray $(L, R)$ represents a slice of the original array starting from the element indexed L to the element indexed R.



**Note**:

* $1 \le n \le 2000$.
* Elements in the given array will be in range `[-1,000,000, 1,000,000]`.



## 题目解读

&emsp;给定数组找到三元组索引$(i,j,k)$，其中$i$、$k$不能是头尾位置，$j$不能是$i$的后继和$k$的前驱，其子区间元素之和相等。

```java
class Solution {
    public boolean splitArray(int[] nums) {

    }
}
```

## 程序设计

* 最初步的想法是计算前缀和，然后三层循环计算比较，但是会超时。

```java
class Solution {
    public boolean splitArray(int[] nums) {
        if(nums == null || nums.length < 7) {
            return false;
        }
        // 计算前缀和
        int[] preSum = new int[nums.length + 1];
        for(int i = 0; i < nums.length; i++) {
            preSum[i + 1] = nums[i] + preSum[i];
        }
        // 遍历比较
        int sum1 = 0, sum2 = 0, sum3 = 0;
        for(int i = 1; i < nums.length - 5; i++) {
            sum1 = preSum[i];
            for(int j = i + 2; j < nums.length - 3; j++) {
                sum2 = preSum[j] - sum1 - nums[i];
                if(sum1 != sum2) {
                    continue;
                }
                for(int k = j + 2; k < nums.length - 1; k++) {
                    sum3 = preSum[k] - preSum[j] - nums[j];
                    if(sum1 != sum3) {
                        continue;
                    }
                    if(sum1 == preSum[nums.length] - preSum[k] - nums[k]) {
                        return true;
                    }
                }
            }
        }
        return false;
    }
}
```

* 参考官方解题，在原来基础上引入集合，记录前半段满足条件的和，然后遍历后半段比较。

```java
class Solution {
    public boolean splitArray(int[] nums) {
        if(nums == null || nums.length < 7) {
            return false;
        }
        // 计算前缀和
        int[] preSum = new int[nums.length + 1];
        for(int i = 0; i < nums.length; i++) {
            preSum[i + 1] = nums[i] + preSum[i];
        }

        Set<Integer> sum = new HashSet<>();
        // 从j开始遍历，而不是i
        for(int j = 3; j < nums.length - 3; j++) {
            for(int i = 1; i < j - 1; i++) {
                // 加入集合
                if(preSum[i] == preSum[j] - preSum[i] - nums[i]) {
                    sum.add(preSum[i]);
                }
            }
            for(int k = j + 2; k < nums.length - 1; k++) {
                int temp = preSum[nums.length] - preSum[k] - nums[k];
                if(preSum[k] - preSum[j] - nums[j] ==  temp && sum.contains(temp)) {
                    return true;
                }
            }
            sum.clear();
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：128ms，在所有java提交中击败了31.51%的用户。

内存消耗：39.6MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;见上。