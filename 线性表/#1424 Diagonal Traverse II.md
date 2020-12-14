[toc]

Given a list of lists of integers, `nums`, return all elements of `nums` in diagonal order as shown in the below images.



**Example 1**:

<img src="..\images\#1424_exp1.png" style="zoom:120%;" />

```
Input: nums = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,4,2,7,5,3,8,6,9]
```

**Example 2**:

<img src="..\images\#1424_exp2.png" style="zoom:100%;" />

```
Input: nums = [[1,2,3,4,5],[6,7],[8],[9,10,11],[12,13,14,15,16]]
Output: [1,6,2,8,7,3,9,4,12,10,5,13,11,14,15,16]
```

**Example 3**:

```
Input: nums = [[1,2,3],[4],[5,6,7],[8],[9,10,11]]
Output: [1,4,2,5,3,8,6,9,7,10,11]
```

**Example 4**:

```
Input: nums = [[1,2,3,4,5,6]]
Output: [1,2,3,4,5,6]
```



**Constraints**:

* $1 \le \text{nums.length} \le 10^5$
* $1 \le \text{nums[i].length} \le 10^5$
* $1 \le \text{nums[i][j]} \le 10^9$
* There at most $10^5$ elements in `nums`.



## 题目解读

&emsp;给定列表，顺序返回对角线上的整数。

```java
class Solution {
    public int[] findDiagonalOrder(List<List<Integer>> nums) {

    }
}
```

## 程序设计

* 对于坐标$(i,j)$，观察到对于所有的$i + j$都在同一条线上，而最左端是起始点，故可从上往下，从左往右遍历，根据$i + j$的值，插入到相应位置的链表头。

```java
class Solution {
    public int[] findDiagonalOrder(List<List<Integer>> nums) {
        int count = 0;
        List<List<Integer>> tmp = new ArrayList<>();
        for (int i = 0; i < nums.size(); i++) {
            for (int j = 0; j < nums.get(i).size(); j++) {
                if (tmp.size() <= i + j) tmp.add(new LinkedList<>());
                tmp.get(i + j).add(0, nums.get(i).get(j));
                count++;
            }
        }

        int[] res = new int[count];
        int idx = 0;
        for (List<Integer> list : tmp) {
            for (int num : list) {
                res[idx++] = num;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，$N$为链表元素总和。

执行用时：30 ms, 在所有 Java 提交中击败了90.94%的用户

内存消耗：63 MB, 在所有 Java 提交中击败了77.35%的用户。

## 官方解题

&emsp;暂无，密切关注。
