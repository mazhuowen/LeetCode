[toc]

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the `candidate` numbers sums to `target`.

Each number in `candidates` may only be used **once** in the combination.

Note:

* All numbers (including `target`) will be positive integers.
* The solution set must not contain duplicate combinations.



## 题目解读

&emsp;与[#39 Combination Sum](./#39 Combination Sum.md)不同，每个元素只能选一次。

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        
    }
}
```

## 程序设计

* 由于每个元素只能选一次，其次输出不能有重复组合，首先可以将数组排序，这样每次回溯时，只要不重复回溯其后的相同元素就可以不造成重复。

```java
class Solution {
    List<List<Integer>> res;

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        res = new LinkedList<>();
        if (candidates == null || candidates.length == 0) return res;

        // 排序，方便后面去重及终止判断
        Arrays.sort(candidates);
        combinationSum2(candidates, 0, target, new ArrayList<>());
        return res;
    }

    private void combinationSum2(int[] candidates, int start, int target, List<Integer> temp) {
        if (target == 0) {
            res.add(new ArrayList<>(temp));
            return;
        }
        for (int i = start; i < candidates.length; i++) {
            // 由于是有序的，后面的都不满足，直接返回
            if (candidates[i] > target) return;
            // 当存在相同的数字时，只需试探第一个数字，因为第一个数字包含了后面数字的情况，避免重复
            if (i > start && candidates[i] == candidates[i - 1]) continue;

            // 试探
            temp.add(candidates[i]);
            combinationSum2(candidates, i + 1, target - candidates[i], temp);
            // 回溯
            temp.remove(temp.size() - 1);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度最坏为$O(N!)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了94.86%的用户。

内存消耗：39.6MB，在所有java提交中击败了18.29%的用户。

## 官方解题

&emsp;暂无，密切关注。