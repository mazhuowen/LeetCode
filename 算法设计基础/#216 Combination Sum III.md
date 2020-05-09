[toc]

Find all possible combinations of **k** numbers that add up to a number **n**, given that only numbers from $1$ to $9$ can be used and each combination should be a unique set of numbers.



Note:

* All numbers will be positive integers.
* The solution set must not contain duplicate combinations.



## 题目解读

&emsp;找出$1 \sim 9$中的$k$个数，使得其和为$n$。

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {

    }
}
```

## 程序设计

* 由于是组合，按数字顺序回溯。

```java
class Solution {
    List<List<Integer>> res;

    public List<List<Integer>> combinationSum3(int k, int n) {
        res = new LinkedList<>();
        if (n <= 1 || k <= 0) return res;

        // 起始数字为i回溯
        for (int i = 1; i <= 10 - k; i++) {
            combinationSum3(new ArrayList<>(), i, k, n);
        }
        return res;
    }

    private void combinationSum3(List<Integer> list, int num, int k, int target) {
        // 不符合要求
        if (target < 0 || k <= 0) return;

        // 试探当前数字及之后的组合
        target -= num;
        k--;
        list.add(num);
        // 找到可行解
        if (target == 0 && k == 0) {
            res.add(new ArrayList<>(list));
        } 
        // 可继续尝试
        else if (target > num && k > 0) {
            for (int i = num + 1; i <= 10 - k; i++) {
                combinationSum3(list, i, k, target);
            }
        }
        
        // 回溯
        list.remove(list.size() - 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(C_9^k)$，空间复杂度为$O(C_9^k)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.2MB，在所有java提交中击败了9.09%的用户。

## 官方解题

&emsp;暂无，密切关注。