[toc]

Given an array equations of strings that represent relationships between variables, each string `equations[i]` has length `4` and takes one of two different forms: `"a==b"` or `"a!=b"`.  Here, `a` and `b` are lowercase letters (not necessarily different) that represent one-letter variable names.

Return `true` if and only if it is possible to assign integers to variable names so as to satisfy all the given equations.



Note:

* $1 \le \text{equations.length} \le 500$
* $\text{equations[i].length} == 4$
* `equations[i][0]` and `equations[i][3]` are lowercase letters
* `equations[i][1]` is either `'='` or `'!'`
* `equations[i][2]` is `'='`



## 题目解读

&emsp;给定一组有关联的表达式，判定是否成立。

```java
class Solution {
    public boolean equationsPossible(String[] equations) {

    }
}
```

## 程序设计

* 由于等式具有不相交集的特性，而不等式不具有，可以维护等式，然后判断不等式是否与等式矛盾。

```java
class Solution {
    public boolean equationsPossible(String[] equations) {
        if(equations == null || equations.length == 0) return true;
        // 分别保存相等和不等的关系
        DisJoint disJoint = new DisJoint(26);
        // 先维护相等关系
        for(String express : equations) {
            char param1 = express.charAt(0);
            char param2 = express.charAt(3);
            if(express.charAt(1) == '=') {
                disJoint.union(disJoint.find(param1 - 'a'), disJoint.find(param2 - 'a'));
            }
        }
        // 再检查不相等表达式
        for(String express : equations) {
            char param1 = express.charAt(0);
            char param2 = express.charAt(3);
            // 存在相等关系，与表达式矛盾
            if(express.charAt(1) == '!' && disJoint.find(param1 - 'a') == disJoint.find(param2 - 'a')) return false;
        }
        return true;
    }
}

class DisJoint {
    private int size;
    private int[] tree;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        Arrays.fill(tree, -1);
    }
    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("not a root");
        if(root1 == root2) return;

        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) return val;
        return tree[val] = find(tree[val]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38MB，在所有java提交中击败了11.29%的用户。

## 官方解题

&emsp;同上。