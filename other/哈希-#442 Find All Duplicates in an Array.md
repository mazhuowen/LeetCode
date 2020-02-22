[toc]

Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

Could you do it without extra space and in O(n) runtime?



## 题目解读

&emsp;给定数组，其中的数字不超过数组长度$n$，找出所有重复的数。题目要求时间复杂度为$O(N)$，空间复杂度为$O(1)$。

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {

    }
}
```

## 程序设计

* 注意到数组长度为$n$，里面的数字都是$1 \le \text{num} \le n$。可以联想到如果数字不重复，每个数组下标正好对应一个值，这个值等于下标加一。
* 考虑到重复情况，数字还是$n$个，但是数字会少于$n$个，可以在遍历时将数字放到相应的位置，如果遇到重复数字，放到缺失的数字索引位置，这样再次遍历发现索引和值对不起来的结果就是重复的数字。

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        // 将除了重复数字之外的其它数字放到正确的位置
        for(int i = 0; i < nums.length; i++) {
            // 数字与下标对不起来
            while(nums[i] != i + 1) {
                int temp = nums[nums[i] - 1];
                // 重复值，停止交换，避免死循环
                if(temp == nums[i]) {
                    break;
                }
                // 交换
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
            }
        }
        // 再次遍历检查
        List<Integer> res = new LinkedList<>();
        for(int i = 0; i < nums.length; i++) {
            // 不相符，则是重复值
            if(nums[i] != i + 1) {
                res.add(nums[i]);
            }
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了93.61%的用户。

内存消耗：48.8MB，在所有java提交中击败了27.64%的用户。

## 官方解题

&emsp;暂无，密切关注。