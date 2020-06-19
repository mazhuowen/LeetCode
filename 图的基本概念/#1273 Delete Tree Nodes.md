[toc]

A tree rooted at node $0$ is given as follows:

* The number of nodes is `nodes`;
* The value of the `i`-th node is `value[i]`;
* The parent of the `i`-th node is `parent[i]`.

Remove every subtree whose sum of values of nodes is zero.

After doing so, return the number of nodes remaining in the tree.



**Constraints**:

* $1 \le \text{nodes} \le 10^4$
* $-10^5 \le \text{value[i]} \le 10^5$
* $\text{parent.length} == \text{nodes}$
* $\text{parent[0]} == -1$ which indicates that $0$ is the root.



## 题目解读

&emsp;给定数组表示的树，将子树和为0的子树移除，求最后保留的节点数目。

```java
class Solution {
    public int deleteTreeNodes(int nodes, int[] parent, int[] value) {

    }
}
```

## 程序设计

* 最基本的思路是建树，然后进行统计。

```java
class Solution {
    public int deleteTreeNodes(int nodes, int[] parent, int[] value) {
        Node root = buildTree(nodes, parent, value);
        return nodes - zeroSum(root);
    }

    private Node buildTree(int nodes, int[] parent, int[] value) {
        Node[] tree = new Node[nodes];
        for (int i = 0; i < nodes; i++) {
            int p = parent[i];
            int val = value[i];
            if (p != -1 && tree[p] == null) tree[p] = new Node(-1);
            
            if (tree[i] == null) tree[i] = new Node(val);
            else tree[i].val = val;
			
            // 维护节点关系
            if (p != -1) tree[p].children.add(tree[i]);
        }
        return tree[0];
    }

    // 移除子树和为0的节点数目
    private int zeroSum(Node root) {
        if (root == null) return 0;

        int count = 0;
        for (Node child : root.children) {
            int c = zeroSum(child);
            if (c != 0) count += c;
			
            // 当前节点加入子树的和
            root.val += child.val;
            // 当前节点统计子树的数目
            root.subNum += child.subNum;
        }
        // 如果当前子树和为0，则删除数就是子树规模
        if (root.val == 0) count = root.subNum;
        return count;
    }
}

class Node {
    int val;
    // 子树节点数目
    int subNum = 1;
    List<Node> children = new ArrayList<>();

    Node(int val) {
        this.val = val;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了63.64%的用户。

内存消耗：43.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了构建图进行深度优先搜索的思路，进一步的提供了拓扑排序的思路。由于本题中的树子结点指向父节点，构建树然后后序遍历统计的过程可借鉴拓扑排序，从入度为$0$节点开始。

```java
class Solution {
    public int deleteTreeNodes(int nodes, int[] parent, int[] value) {
        // 入度
        int[] inDegree = new int[nodes];
        for (int p : parent) {
            if (p == -1) continue;
            inDegree[p]++;
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < nodes; i++) {
            if (inDegree[i] == 0) queue.add(i);
        }

        // 子树节点数目
        int[] subNum = new int[nodes];
        Arrays.fill(subNum, 1);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            // 子树和为0，移除，节点为0
            if (value[cur] == 0) subNum[cur] = 0;

            if (parent[cur] == -1) continue;
            
            value[parent[cur]] += value[cur];
            subNum[parent[cur]] += subNum[cur];

            if (--inDegree[parent[cur]] == 0) queue.add(parent[cur]);
        }

        return subNum[0];
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：7ms，在所有java提交中击败了81.82%的用户。

内存消耗：40.7MB, 在所有java提交中击败了100.00%的用户。