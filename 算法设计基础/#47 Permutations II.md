[toc]

Given a collection of numbers that might contain duplicates, return all possible unique permutations.



## 题目解读

&emsp;与[#46 Permutations](./#46 Permutations.md)不同，本题存在重复数字。

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {

    }
}
```

## 程序设计

* 先排序，在回溯时本轮不再遍历相同数字，避免造成重复。

```java
class Solution {
    List<List<Integer>> res;
    boolean[] flag;

    public List<List<Integer>> permuteUnique(int[] nums) {
        res = new LinkedList<>();
        if (nums == null || nums.length == 0) return res;

        flag = new boolean[nums.length];
        // 排序消除重复
        Arrays.sort(nums);
        permuteUnique(nums, new LinkedList<>());
        return res;
    }

    private void permuteUnique(int[] nums, List<Integer> temp) {
        boolean end = true;
        for (int i = 0; i < nums.length; i++) {
            // 已加入序列或者在本轮遍历中与前一个数字一致，则跳过
            // 本轮中前一个数字回溯完，flag必然是false；如果不是同一轮flag则是true
            if (flag[i] || (i > 0 && !flag[i - 1] && nums[i] == nums[i - 1])) continue;

            end = false;
            flag[i] = true;
            temp.add(nums[i]);
            permuteUnique(nums, temp);
            flag[i] = false;
            temp.remove(temp.size() - 1);
        }

        if (end) {
            res.add(new LinkedList<>(temp));
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N!)$，空间复杂度为$O(N!)$。

执行用时：2ms，在所有java提交中击败了87.81%的用户。

内存消耗：40.1MB，在所有java提交中击败了26.19%的用户。

## 官方解题

&emsp;暂无，密切关注。