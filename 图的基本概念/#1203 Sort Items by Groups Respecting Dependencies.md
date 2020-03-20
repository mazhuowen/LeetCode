[toc]

There are `n` items each belonging to zero or one of `m` groups where `group[i]` is the group that the `i`-th item belongs to and it's equal to `-1` if the `i`-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

- The items that belong to the same group are next to each other in the sorted list.
- There are some relations between these items where `beforeItems[i]` is a list containing all the items that should come before the `i`-th item in the sorted array (to the left of the `i`-th item).

Return any solution if there is more than one solution and return an **empty list** if there is no solution.



**Constraints:**

- $1 \le m \le n \le 3*10^4$
- $\text{group.length} == \text{beforeItems.length} == n$
- $-1 \le \text{group[i]} \le m-1$
- $0 \le \text{beforeItems[i].length} \le n-1$
- $0 \le \text{beforeItems[i][j]} \le n-1$
- $i != \text{beforeItems[i][j]}$
- $\text{beforeItems[i]}$does not contain duplicates elements.



## 题目解读

&emsp;

```java
class Solution {
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {

    }
}
```

## 程序设计



```java

```

## 性能分析



## 官方解题

