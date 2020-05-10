[toc]

Given a collection of integers that might contain duplicates, **nums**, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.



## 题目解读

&emsp;规定所有排列，不能有重复。

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        
    }
}
```

## 程序设计

* 注意题目要求不能有重复，其次是排列不是组合，故需要排序跳过重复值。

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();

        if (nums == null || nums.length == 0) return res;

        Arrays.sort(nums);
        subsetsWithDup(nums, -1, new ArrayList<>(), res);
        return res;
    }

    private void subsetsWithDup(int[] nums, int start, List<Integer> temp, List<List<Integer>> res) {
        res.add(new ArrayList<>(temp));

        Integer pre = null;
        for (int i = start + 1; i < nums.length; i++) {
            // 避免重复回溯
            if (pre != null && pre == nums[i]) continue;
            pre = nums[i];
            // 试探
            temp.add(pre);
            subsetsWithDup(nums, i, temp, res);
            // 回溯
            temp.remove(temp.size() - 1);
        }
    } 
}
```

## 性能分析

&emsp;时间复杂度为$O(N!)$，空间复杂度为$O(N!)$。

执行用时：1ms，在所有java提交中击败了99.97%的用户。

内存消耗：40.2MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;暂无，密切关注。