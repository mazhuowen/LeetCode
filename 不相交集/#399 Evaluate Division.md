[toc]

Equations are given in the format `A / B = k`, where `A` and `B` are variables represented as strings, and `k` is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return `-1.0`.

The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.



## 题目解读

&emsp;给定表达式和值，计算新的表达式，如果不存在则返回`-1`。

```java
class Solution {
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {

    }
}
```

## 程序设计

* 当两个变量没有关联或不存在时，返回`-1`。这可以使用不相交集实现，每次将表达式的两个变量合并。难点在于同一集合的数值怎么计算关联。首先联想到不相交集是树的双亲表达形式，如果每个结点保存父节点和当前结点的商，则可以计算出根和每个结点的商，进而可以计算出同一集合任意两个结点的商。
* 难点在于当得到一个表达式后，怎么加入现有的集合。假设表达式为`a/b = val`，如果直接将`a`作为`b`的父节点，并存储`val`到`b`的结点，则存在的问题是对于除法的另一个传递性，即已知`c/d`、`e/d`则可知`c/e`，如果采用上述思路，则会漏掉这种情况。更一般的，将`a`、`b`各自树的根节点合并，但问题是这两个根节点的商是多少？假设根与`a`、`b`的商分别为`s1`、`s2`，则`root1/a = s1`，`root2/b = s2`，可知`root1/root2 = a*s1 / b*s2`。
* 做了相应改变的不相交集类如下：

```java
class DisJoint {
    private int[] tree;
    // 记录与父节点的商
    private double[] result;
    private int size;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        this.result = new double[size];
        // 填充-1
        Arrays.fill(tree, -1);
        // 商初始填充1
        Arrays.fill(result, 1.0D);
    }
    // 合并两个数（转化为合并根及两个根的商）
    public void union(int num1, int num2, double val) {
        int root1 = find(num1);
        int root2 = find(num2);
        // roo1/num1的值
        double d1 = calcuRoot(num1);
        // root2/num2的值
        double d2 = calcuRoot(num2);
        // 将树2合并到树1
        if(tree[root1] <= tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
            // root1/root2 = d1 * num1/d2 * num2
            result[root2] = d1 / d2 * val;
        } else {
            tree[root2] += tree[root1];
            tree[root1] = root2;
            // root2/root1 = d2 * num2/d1 * num1
            result[root1] = d2 / d1 / val;
        }
        size--;
    }
    // 路径压缩
    public int find(int idx) {
        if(tree[idx] < 0) return idx;
        // 重新计算（由于路径压缩会改变父节点，在压缩前重新计算路径上的乘积）
        result[idx] = calcuRoot(idx);
        return tree[idx] = find(tree[idx]);
    }
    // 计算两数之商
    public double calcuResult(int num1, int num2) {
        // 不连通
        if(find(num1) != find(num2)) {
            return -1.0D;
        }
        // r1 = root/num1,r2 = root/num2 -> num1/num2 = r2/r1
        double r1 = calcuRoot(num1);
        double r2 = calcuRoot(num2);
        return r2 / r1;
    }
    // 计算根节点到当前值的商
    private double calcuRoot(int num) {
        if(tree[num] < 0) return result[num];
        return result[num] * calcuRoot(tree[num]);
    }

    public int size() {
        return size;
    }
}
```

* 得到上述类后，只需将变量映射到连续的数字，然后进行合并即可。

```java
class Solution {
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        if(equations == null || equations.size() != values.length) {
            throw new IllegalArgumentException("invalid param");
        }
        // 记录字符和索引映射
        Map<String, Integer> idxMap = new HashMap<>();
        int idx = 0;
        // 元素的数目不超过表达式的两倍
        DisJoint disJoint = new DisJoint(2 * values.length);
        // 遍历表达式，合并计算
        for(int i = 0; i < values.length; i++) {
            String num1 = equations.get(i).get(0);
            if(idxMap.get(num1) == null) idxMap.put(num1, idx++);
            String num2 = equations.get(i).get(1);
            if(idxMap.get(num2) == null) idxMap.put(num2, idx++);
            // 维护商
            disJoint.union(idxMap.get(num1), idxMap.get(num2), values[i]);
        }
        double[] res = new double[queries.size()];
        for(int i = 0; i < res.length; i++) {
            // 不存在的变量，返回-1
            String num1 = queries.get(i).get(0);
            if(idxMap.get(num1) == null) {
                res[i] = -1.0D;
                continue;
            }
            String num2 = queries.get(i).get(1);
            if(idxMap.get(num2) == null) {
                res[i] = -1.0D;
                continue;
            }
            // 计算
            res[i] = disJoint.calcuResult(idxMap.get(num1), idxMap.get(num2));

        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + M)$，时间复杂度为$O(N)$，其中$N$为已知表达式数目，$M$为待计算数目。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.9MB, 在所有java提交中击败了5.88%的用户。

## 官方解题

&emsp;暂无，密切关注。