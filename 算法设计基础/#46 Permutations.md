[toc]

Given a collection of **distinct** integers, return all possible permutations.



## 题目解读

&emsp;给定无重复数字的数组，给出所有的排列组合。

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {

    }
}
```

## 程序设计

* 回溯的基本问题，需要额外的数组记录是否访问过。

```java
class Solution {
    List<List<Integer>> res;
    boolean[] flag;

    public List<List<Integer>> permute(int[] nums) {
        res = new LinkedList<>();
        if (nums == null || nums.length == 0) return res;

        flag = new boolean[nums.length];
        permute(nums, new ArrayList<>());
        return res;
    }

    private void permute(int[] nums, List<Integer> temp) {
        boolean end = true;
        for (int i = 0; i < nums.length; i++) {
            // 已加入序列，继续
            if (flag[i]) continue;
            // 未结束，结束标识置为false
            end = false;

            // 试探
            flag[i] = true;
            temp.add(nums[i]);
            permute(nums, temp);
            // 回溯
            flag[i] = false;
            temp.remove(temp.size() - 1);
        }
        // 如果序列已结束，则加入
        if (end) {
            res.add(new ArrayList<>(temp));
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N!)$，空间复杂度为$O(N!)$。

执行用时：2ms，在所有java提交中击败了76.66%的用户。

内存消耗：39.9MB，在所有java提交中击败了5.24%的用户。

## 官方解题

&emsp;思路同上，实现细节不同。