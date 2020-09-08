[toc]

Given an array A of non-negative integers, the array is squareful if for every pair of adjacent elements, their sum is a perfect square.

Return the number of permutations of A that are squareful.  Two permutations `A1` and `A2` differ if and only if there is some index $i$ such that $\text{A1[i]} \ne \text{A2[i]}$.



**Note:**

* $1 \le \text{A.length} \le 12$
* $0 \le \text{A[i]} \le 10^9$



## 题目解读

&emsp;定义正方形数组为相邻的元素之和是平方值，给定数组求这样的数组的排列组合数目。

```java
class Solution {
    public int numSquarefulPerms(int[] A) {

    }
}
```

## 程序设计

* 最基本的思路是暴力回溯，由于长度较短且前后元素需要满足一定条件，可剪枝加速；对于重复排列组合，可通过尝试当前数值后遍历尝试下一个不同数字来解决。

```java
class Solution {
    private int count = 0;

    public int numSquarefulPerms(int[] A) {
        Arrays.sort(A);
        boolean[] visited = new boolean[A.length];
        backTracing(A, visited, 0, -1);
        return count;
    }

    private void backTracing(int[] nums, boolean[] visited, int idx, int pre) {
        if (idx == nums.length) {
            count++;
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            // 已访问或无法组成平方数，跳过
            if (visited[i] || !(idx == 0 || isSquare(pre + nums[i]))) continue;
            visited[i] = true;
            backTracing(nums, visited, idx + 1, nums[i]);
            visited[i] = false;
            // 遍历直到数值不同，排重
            while (i + 1 < nums.length && nums[i] == nums[i + 1]) i++;
        }
    }

    private boolean isSquare(int num) {
        int d = (int)Math.sqrt(num);
        return d * d == num;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N!)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.7MB，在所有java提交中击败了96.59%的用户。

## 官方解题

&emsp;官方采用构图，然后查找哈密顿路径数目的思路。