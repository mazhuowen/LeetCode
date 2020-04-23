[toc]

Given a non-negative integer `numRows`, generate the first `numRows `of Pascal's triangle.

<img src="../images/#118.gif"  />



## 题目解读

&emsp;生成杨辉三角。

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {

    }
}
```

## 程序设计

* 后一行的值是前一行的值和左侧值之和。

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> res = new LinkedList<>();
        List<Integer> preRow = new ArrayList<>();
        for (int i = 0; i < numRows; i++) {
            List<Integer> curRow = new ArrayList<>();
            // 开头添加1
            curRow.add(1);
            // 填充中间值
            for (int j = 1; j < i; j++) {
                curRow.add(preRow.get(j - 1) + preRow.get(j));
            }
            // 结尾添加1
            if (i > 0) curRow.add(1);
            res.add(curRow);
            // 迭代
            preRow = curRow;
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：1ms，在所有java提交中击败了83.31%的用户。

内存消耗：37.4MB，在所有java提交中击败了9.09%的用户。

## 官方解题

&emsp;同上。