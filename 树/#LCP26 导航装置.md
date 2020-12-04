[toc]

小扣参加的秋日市集景区共有 $N$ 个景点，景点编号为 $1 \sim N$。景点内设有 $N-1$ 条双向道路，使所有景点形成了一个二叉树结构，根结点记为 `root`，景点编号即为节点值。

由于秋日市集景区的结构特殊，游客很容易迷路，主办方决定在景区的若干个景点设置导航装置，按照所在景点编号升序排列后定义装置编号为 $1 \sim M$。导航装置向游客发送数据，数据内容为列表 `[游客与装置 1 的相对距离,游客与装置 2 的相对距离,...,游客与装置 M 的相对距离]`。由于游客根据导航装置发送的信息来确认位置，因此主办方需保证游客在每个景点接收的数据信息皆不相同。请返回主办方最少需要设置多少个导航装置。



**示例 1**：

```
输入：root = [1,2,null,3,4]

输出：2

解释：在景点 1、3 或景点 1、4 或景点 3、4 设置导航装置。
```

<img src="..\images\#lcp26_exp1.png" style="zoom: 50%;" />



**示例 2**：

```
输入：root = [1,2,3,4]

输出：1

解释：在景点 3、4 设置导航装置皆可。
```

<img src="..\images\#lcp26_exp2.png" style="zoom: 50%;" />



**提示**：

* $2 \le N \le 50000$
* 二叉树的非空节点值为 $1 \sim N$ 的一个排列。



## 题目解读

&emsp;给定二叉树，求设置最少的导航装置，使得到每个节点的信息不同。导航装置到节点的信息为到该节点的距离。

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
    public int navigation(TreeNode root) {

    }
}
```

## 程序设计

* 经观察和分析可得，对于具有三个分支的节点，只要有两个分支安装导航则每个位置都可辨；
* 由于根节点比较特殊，首先分别处理左右子树；从叶节点开始`19-28-29`构成第一个三分叉，由上述分析可知需要两个导航，但是难点在于`19`的祖先路径还未遍历到，无法确定是否分配一个在祖先路径上；可以先在子树中分配一个导航，比如`28`，预留一个以防后续在祖先路径分配；继续遍历，到了`13-18-19`第二个三岔口，此时`19`这支子树已经有一个导航，仍然预留一个在祖先路径中；右侧子树同理，假设分配导航到`30`，`32`，`23`，`25`，从而`5-7-8`三岔口无需预留导航，因为两个子树都有导航；最后需要处理根节点，此时左子树还需要一个导航，对右侧提供一个导航；而右子树无需导航，对左子树提供一个导航，从而相互都得到满足，共计需要$5$个导航；
* 上述模拟可得，自底到顶遍历判断三岔口，如果两个子树都没有导航，则分配一个并预留一个，且需要注意这个三岔口对外可看作提供一个导航；如此直到根节点，然后判断是否满足预留条件。对于没有三岔口的情况，即是链条的情况，则需预留一个导航。

<img src="..\images\#lcp26.PNG" style="zoom:80%;" />

```java
class Solution {
    public int navigation(TreeNode root) {
        if (root == null) return 0;
        int[] left = allocation(root.left);
        int[] right = allocation(root.right);

        int res = left[1] + right[1];
        // 左右子树无法抵消，还需要在根节点安装一个
        if (left[0] < right[2] || left[2] > right[0]) res++;
        return res;
    }

    // 数组中元素依次表示子树是否安装导航、安装数量、需要安装数量
    private int[] allocation(TreeNode root) {
        if (root == null) return new int[]{0, 0, 0};

        int[] left = allocation(root.left);
        int[] right = allocation(root.right);

        // 两个子节点（与父节点构成三分支）
        if (root.left != null && root.right != null) {
            // 左右都装了导航，对于三分支无需再额外安装导航
            if ((left[0] & right[0]) == 1) {
                return new int[]{1, left[1] + right[1], 0};
            } 
            // 只有一个子树安装了导航，还需再安装一个
            else if ((left[0] | right[0]) == 1) {
                return new int[]{1, left[1] + right[1], 1};
            }
            // 都未安装，则先安装任意一个分支的叶子节点，还需再安装一个
            else {
                return new int[]{1, left[1] + right[1] + 1, 1};
            }
        }
        // 叶子节点，暂不安装，需要安装一个
        else if (root.left == null && root.right == null) {
            return new int[]{0, 0, 1};
        }
        // 一个子节点（链条情况），预留一个导航
        else {
            return new int[]{left[0] | right[0], left[1] + right[1], left[2] | right[2]};
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：17 ms, 在所有 Java 提交中击败了20.69%的用户

内存消耗：56.5 MB, 在所有 Java 提交中击败了13.79%的用户。

## 官方解题

&emsp;暂无，密切关注。