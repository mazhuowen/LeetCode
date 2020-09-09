[toc]

If the depth of a tree is smaller than `5`, then this tree can be represented by a list of three-digits integers.

For each integer in this list:

* The hundreds digit represents the depth `D` of this node, $1 \le D \le 4$.
* The tens digit represents the position `P` of this node in the level it belongs to, $1 \le P \le 8$. The position is the same as that in a full binary tree.
* The units digit represents the value `V` of this node, $0 \le V \le 9$.

Given a list of `ascending` three-digits integers representing a binary tree with the depth smaller than 5, you need to return the sum of all paths from the root towards the leaves.

It's guaranteed that the given list represents a valid connected binary tree.



## 题目解读

&emsp;采用三位数字表示深度小于$5$的树，百位表示深度，十位表示在当前层的相对位置，个位表示节点值。给定节点列表，求叶子节点到根节点的所有路径和。

```java
class Solution {
    public int pathSum(int[] nums) {
       
    }
}
```

## 程序设计

* 最基本的思路可以直接构建二叉树，然后遍历计算；也可直接计算，由于百位十位唯一决定一个节点，而且可根据百位得到子节点的层数，根据十位得到子节点的编号，故可由父节点直接得到子节点。
* 可提前将所有节点编号存入字典，遍历过程判断子节点是否存在，存在则迭代计算直到叶节点。

```java
class Solution {
    public int pathSum(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        // 节点（深度和广度组合）与节点值
        Map<Integer, Integer> tree = new HashMap<>();
        for (int num : nums) {
            tree.put(num / 10, num % 10);
        }
        
        // 入队根节点
        Queue<Integer> queue = new LinkedList<>();
        queue.add(11);

        int res = 0;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            // 层数和广度
            int level = cur / 10, no = cur % 10;
            // 左右子节点
            int left = (level + 1) * 10 + no * 2 - 1, right = left + 1;

            boolean isLeaf = true;
            if (tree.containsKey(left)) {
                isLeaf = false;
                tree.put(left, tree.get(cur) + tree.get(left));
                queue.add(left);
            }
            if (tree.containsKey(right)) {
                isLeaf = false;
                tree.put(right, tree.get(cur) + tree.get(right));
                queue.add(right);
            }
            if (isLeaf) res += tree.get(cur);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了21.49%的用户。

内存消耗：37.5MB，在所有java提交中击败了42.31%的用户。

## 官方解题

&emsp;官方思路类似。