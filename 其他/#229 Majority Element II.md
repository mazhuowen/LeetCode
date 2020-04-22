[toc]

Given an integer array of size $n$, find all elements that appear more than $\frac{n}{3}$ times.



Note: The algorithm should run in linear time and in $O(1)$ space.



## 题目解读

&emsp;给出数组中超过三分之一数量的数。

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {

    }
}
```

## 程序设计

* 参考寻找众数的摩尔投票，当扩展到寻找超过$\frac{n}{3}$的数时，最多有$2$个候选，主体思路与寻找众数的摩尔投票类似，消除由两个不相等数字对变为三个不相等数字对，即遇到三个不同的数，计数才减少；计数为0则寻找赋值$2$个候选。
* 最后得到的投票计数大于1的候选是可能解，还需要再次遍历验证。

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> res = new LinkedList<>();
        if (nums == null || nums.length == 0) return res;

        int candidate1 = 0, candidate2 = 0, count1 = 0, count2 = 0;
        for (int num : nums) {
            // 候选1被消除，判断重新赋值
            if (count1 == 0 && (count2 == 0 || candidate2 != num)) {
                candidate1 = num;
            } 
            // 候选2被消除，判断重新赋值
            else if (candidate1 != num && count2 == 0) {
                candidate2 = num;
            }
            // 计数
            if (num == candidate1) count1++;
            else if (num == candidate2) count2++;
            // 消除
            else {
                count1--;
                count2--;
            }
        }

        // 验证
        if (count1 > 0 && isValid(candidate1, nums)) res.add(candidate1);
        if (count2 > 0 && isValid(candidate2, nums)) res.add(candidate2);
        return res;
    }

    private boolean isValid(int candidate, int[] nums) {
        int count = 0;
        for (int num : nums) if (num == candidate) count++;
        return count > nums.length / 3;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了50.76%的用户。

内存消耗：43.6MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;暂无，密切关注。