[toc]

Given a set of **distinct** integers, `nums`, return all possible subsets (the power set).



**Note**: The solution set must not contain duplicate subsets.



## 题目解读

&emsp;找出所有的子集。

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {

    }
}
```

## 程序设计

* 由于数组元素是唯一的，探索只要从左至右遍历，便不会重复；每次从后面的元素中选择一个作为下一个组合值。
* 由于包含空集，需要将索引设置到数组长度。

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        
        // 使用nums.length来标识结束
        for (int i  = 0; i <= nums.length; i++) {
            // 以i为开始的元素与其后元素的组合
            subsets(nums, i, new ArrayList<>(), res);
        }
        return res;
    }

    private void subsets(int[] nums, int start, List<Integer> list, List<List<Integer>> res) {
        // 找到可行组合（包括空集）
        if (start >= nums.length) {
            res.add(new ArrayList<>(list));
            return;
        }
        // 尝试
        list.add(nums[start]);
        for (int i = start + 1; i <= nums.length; i++) {
            subsets(nums, i, list, res);
        }
        // 回溯
        list.remove(list.size() - 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(2^N)$。

执行用时：1ms，在所有java提交中击败了99.32%的用户。

内存消耗：39.6MB，在所有java提交中击败了5.45%的用户。

## 官方解题

&emsp;同上，还有其他思路。