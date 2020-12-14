[toc]

The set `S` originally contains numbers from $1$ to $n$. But unfortunately, due to the data error, one of the numbers in the set got duplicated to **another** number in the set, which results in repetition of one number and loss of another number.

Given an array `nums` representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.



**Example 1**:

```
Input: nums = [1,2,2,4]
Output: [2,3]
```


**Note**:

* The given array size will in the range $[2, 10000]$.
* The given array's numbers won't have any order.



## 题目解读

&emsp;给定数组，元素为$1 \sim N$，其中有一个数字重复，对应的另一个数字确实，判断并求解。

```java
class Solution {
    public int[] findErrorNums(int[] nums) {

    }
}
```

## 程序设计

* 由于数字在$1 \sim N$的范围内，故可用数组作为哈比表。

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int[] res = new int[2];
        for (int i = 0; i < nums.length; i++) {
            while (i + 1 != nums[i] && nums[i] != nums[nums[i] - 1]) {
                swap(i, nums[i] - 1, nums);
            }
        }
        // 遍历判断
        for (int i = 0; i < nums.length; i++) {
            if (i + 1 != nums[i]) {
                res[0] = nums[i];
                res[1] = i + 1;
                break;
            }
        }
        return res;
    }

    private void swap(int i, int j, int[] nums) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3 ms, 在所有 Java 提交中击败了70.43%的用户

内存消耗：40 MB, 在所有 Java 提交中击败了66.28%的用户。

## 官方解题

&emsp;官方还提供了亦或求解，需要多次遍历数组。
