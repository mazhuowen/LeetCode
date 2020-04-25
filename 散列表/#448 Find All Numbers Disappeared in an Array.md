[toc]

Given an array of integers where $1 \le \text{a[i]} \le n$ (n = size of array), some elements appear twice and others appear once.

Find all the elements of `[1, n]` inclusive that do not appear in this array.

Could you do it without extra space and in $O(n)$ runtime? You may assume the returned list does not count as extra space.



## 题目解读

&emsp;排查所有缺失的数值。

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        
    }
}
```

## 程序设计

* 如果不考虑空间复杂度，则使用字典记录出现的整数，最后遍历记录没有出现的即可。题目要求空间复杂度为$O(1)$，结合数组只包含$1 \sim n$的数值，可使用数组作为映射表。

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new LinkedList<>();
        if (nums == null || nums.length == 0) return res;

        for (int i = 0; i < nums.length; i++) {
            // 当前位置不相等，且待交换位置也不相等（避免死循环）
            while (nums[i] != i + 1 && nums[i] != nums[nums[i] - 1]) {
                // 交换，直到当前位置相等
                int temp = nums[i];
                nums[i] = nums[temp - 1];
                nums[temp - 1] = temp;
            }
        }
        // 检查不相等的位置
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) res.add(i + 1);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了79.29%的用户。

内存消耗：47.9MB，在所有java提交中击败了79.17%的用户。

## 官方解题

&emsp;官方思路类似，交换处理略有不同。