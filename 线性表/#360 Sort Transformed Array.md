[toc]

Given a **sorted** array of integers nums and integer values a, b and c. Apply a quadratic function of the form $f(x) = ax^2 + bx + c$ to each element x in the array.

The returned array must be in **sorted order**.

Expected time complexity: $O(n)$



## 题目解读

&emsp;排序给定方程计算的有序数组，要求时间复杂度为$O(N)$。

```java
class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {

    }
}
```

## 程序设计

* 最基本的，根据抛物线对称轴来判断，还需注意$a=0$的情况。

```java
class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        if (nums == null || nums.length == 0) return nums;
        int[] res = new int[nums.length];
        if (a == 0 && b == 0) {
            Arrays.fill(res, c);
            return res;
        }
        if (a == 0) {
            if (b > 0) {
                for (int i = 0; i < nums.length; i++) {
                    res[i] = b * nums[i] + c;
                }
            } else {
                for (int i = 0; i < nums.length; i++) {
                    res[nums.length - 1 - i] = b * nums[i] + c;
                }
            }
            return res;
        }

        double axis = -b / 2.0D / a;
        int right = binarySearch(nums, axis), left = right - 1;
        int idx = 0;
        while (right < nums.length && left >= 0) {
            if (a > 0) {
                if (Math.abs(nums[right] - axis) <= Math.abs(axis - nums[left])) {
                    res[idx++] = a * nums[right] * nums[right] + b * nums[right] + c;
                    right++;
                } else {
                    res[idx++] = a * nums[left] * nums[left] + b * nums[left] + c;
                    left--;
                }
            } else {
                if (Math.abs(nums[right] - axis) <= Math.abs(axis - nums[left])) {
                    res[nums.length - 1 - idx++] = a * nums[right] * nums[right] + b * nums[right] + c;
                    right++;
                } else {
                    res[nums.length - 1 - idx++] = a * nums[left] * nums[left] + b * nums[left] + c;
                    left--;
                }
            }
        }

        while (right < nums.length) {
            if (a > 0) res[idx++] = a * nums[right] * nums[right] + b * nums[right++] + c;
            else res[nums.length - 1 - idx++] = a * nums[right] * nums[right] + b * nums[right++] + c;
        }

        while (left >= 0) {
            if (a > 0) res[idx++] = a * nums[left] * nums[left] + b * nums[left--] + c;
            else res[nums.length - 1 - idx++] = a * nums[left] * nums[left] + b * nums[left--] + c;
        }
        return res;
    }

    private int binarySearch(int[] nums, double target) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.39%的用户。

内存消耗：39.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区较为优雅的方式使用双指针，先计算再判断，减少很多判断分支。

```java
class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        if (nums == null || nums.length == 0) return nums;
        int[] res = new int[nums.length];
        int[] temp = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            int x = nums[i];
            temp[i] = a * x * x + b * x + c;
        }
        
        int left = 0, right = nums.length - 1;
        int idx = 0;
        // 左右是最大值
        if (a > 0) {
            while (left <= right) {
                if (temp[left] >= temp[right]) res[nums.length - 1 - idx++] = temp[left++];
                else res[nums.length - 1 - idx++] = temp[right--];
            }
        } 
        // 左右是最小值（包括a=0的情况，此时左边或右边是最小值） 
        else {
            while (left <= right) {
                if (temp[left] <= temp[right]) res[idx++] = temp[left++];
                else res[idx++] = temp[right--];
            }
        }
        return res;
    }
}
```

