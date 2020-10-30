[toc]

Given a binary tree, we install cameras on the nodes of the tree. 

Each camera at a node can monitor **its parent, itself, and its immediate children**.

Calculate the minimum number of cameras needed to monitor all nodes of the tree.



**Note**:

* The number of nodes in the given tree will be in the range `[1, 1000]`.
* **Every** node has value 0.



## 题目解读

&emsp;给定一个二叉树，在树的节点上安装摄像头，节点上的每个摄影头都可以监视**其父对象、自身及其直接子对象。**计算监控树的所有节点所需的最小摄像头数量。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int minCameraCover(TreeNode root) {

    }
}
```

## 程序设计

* 参考官方解题贪心法，思路为自底向上，如果子节点都被覆盖，则当前结点不必安装摄像头，其父节点安装即可；如果父节点为空，当前结点未被覆盖或存在子节点未被覆盖，则在当前结点安装监控。

```java
class Solution {
    // 摄像头计数
    private int count;
    // 摄像头覆盖的结点集合
    private Set<TreeNode> record;

    public int minCameraCover(TreeNode root) {
        count = 0;
        record = new HashSet<>();
        // 在受监控集合中加入空结点
        record.add(null);
        cover(root, null);
        return count;
    }

    private void cover(TreeNode cur, TreeNode parent) {
        if(cur == null) return;
        cover(cur.left, cur);
        cover(cur.right, cur);
        // 当前子节点都被覆盖
        if(record.contains(cur.left) && record.contains(cur.right)) {
            // 如果存在父节点，父节点可添加摄像头或不存在父节点但当前结点也被覆盖
            if(parent != null || record.contains(cur)) return;
            // 不存在父节点，则是根节点，添加摄像头
            count++;
            record.add(cur);
        }
        // 当前子节点不全被覆盖，当前结点添加摄像头
        else {
            count++;
            record.add(cur);
            record.add(cur.left);
            record.add(cur.right);
            record.add(parent);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：3ms，在所有java提交中击败了11.02%的用户。

内存消耗：39.7MB，在所有java提交中击败了22.73%的用户。

## 官方解题

&emsp;官方还提供了动态规划的思路。社区时间性能较好的方法简化了官方的流程，采用返回状态的方式代替集合。

```java
class Solution {
    private int ans = 0;
    
    public int minCameraCover(TreeNode root) {
        if (root == null) return 0;
        if (dfs(root) == 2) ans++;
        return ans;
    }
    
    // 0表示该节点安装了监视器 1表示该节点可观，但没有安装监视器 2表示该节点不可观
    private int dfs(TreeNode node) {
        if (node == null)
            return 1;
        int left = dfs(node.left), right = dfs(node.right); 
        // 子节点未被覆盖，当前结点安装监控
        if (left == 2 || right == 2) {
            ans++;
            return 0;
        } 
        // 有子节点安装监控，则当前结点被覆盖
        else if (left == 0 || right == 0){            
            return 1;
        } 
        // 子节点安装监控覆盖当前结点
        else 
            return 2;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.9MB，在所有java提交中击败了40.91%的用户。