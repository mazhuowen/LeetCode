[toc]

Given a **set** of candidate numbers (`candidates`) (**without duplicates**) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

Note:

* All numbers (including `target`) will be positive integers.
* The solution set must not contain duplicate combinations.



## 题目解读

&emsp;给定一个无重复元素的数组，找出所有可以使数字和为目标值的组合。数组中的数字可以无限制重复被选取。

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        
    }
}
```

## 程序设计

* 首先想到的是回溯尝试所有组合，注意题目中返回不能有重复组合，需要去重。

```java
class Solution {
    private Set<List<Integer>> set;

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new LinkedList<>();
        if (candidates == null || candidates.length == 0) return res;

        // 去除重复组合
        set = new HashSet<>();
        // 回溯
        combinationSum(candidates, target, new LinkedList<>());
        res = new LinkedList<>(set);
        return res;
    }

    private void combinationSum(int[] candidates, int target, List<Integer> temp) {
        // 不符合要求
        if (target < 0) return;
        // 找到一组可行解
        if (target == 0 && !temp.isEmpty()) {
            // 排序，方便去重
            List<Integer> res = new LinkedList<>(temp);
            Collections.sort(res);
            set.add(res);
            return;
        }
        // 尝试所有组合
        for (int num : candidates) {
            temp.add(num);
            combinationSum(candidates, target - num, temp);
            // 回溯
            temp.remove(temp.size() - 1);
        }
    }
}
```

* 事实上在回溯的循环中，如果待加入值大于目标值，则不必在该值上试探；其次上线的方法需要试探所有可能的组合，包括重复组合，最后又要对这些重复组合去重，由于题目中限定没有没有重复元素，可以排序，从值较低的元素进行遍历回溯，每次试探新的值，不能比当前值小，这样就解决了重复组合的问题。优化如下：

```java
class Solution {
    private List<List<Integer>> res;

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        res = new LinkedList<>();
        if (candidates == null || candidates.length == 0) return res;
        // 排序方便终止无效回溯及排除重复
        Arrays.sort(candidates);
        // 回溯
        combinationSum(candidates, target, 0, new LinkedList<>());
        return res;
    }

    // start为本次试探的起始位置，避免尝试重复组合
    private void combinationSum(int[] candidates, int target, int start, List<Integer> temp) {
        // 不符合要求
        if (target < 0) return;
        // 找到一组可行解
        if (target == 0 && !temp.isEmpty()) {
            res.add(new LinkedList<>(temp));
            return;
        }
        // 尝试所有组合
        for (int i = start; i < candidates.length && candidates[i] <= target; i++) {
            temp.add(candidates[i]);
            combinationSum(candidates, target - candidates[i], i, temp);
            // 回溯
            temp.remove(temp.size() - 1);
        }
    }
}
```

## 性能分析

&emsp;回溯时间复杂度最坏为$O(N^M)$，空间复杂度为$O(M)$，此时数组为`[1,1,…,1]`有$N$个，而目标值为$M$。

执行用时：91ms，在所有java提交中击败了5.00%的用户。

内存消耗：40.4MB，在所有java提交中击败了7.31%的用户。

&emsp;优化后最坏时间复杂度为$O(N - M)!$，空间复杂度为$O(M)$。

执行用时：3ms，在所有java提交中击败了94.94%的用户。

内存消耗：39.7MB，在所有java提交中击败了14.21%的用户。

## 官方解题

&emsp;暂无，密切关注。