[toc]

## 二叉树

&emsp;二叉树是最普遍的树，其特性决定了递归是其最通用的思路，二叉树的问题几乎都围绕遍历操作。二叉树遍历分为前序、中序、后序、层次遍历，除了层次遍历通常使用队列实现，其它均可用递归优雅实现，对应的非递归实现需要记录额外的入栈次数。而前序遍历实质上是自顶向下的深度搜索，后续遍历是自底向上的遍历。通过自顶向下递归可以计算树的深度、树的路径、最大连续子序列等，通过自底向上可以递归计算子节点，如结点的路径之和、公共最小祖先结点等。

&emsp;遍历除了遍历树的结点，还可以确定一棵树。当已知前序遍历、中序遍历或后序遍历、中序遍历序列便可以唯一确定一棵树；一些变种前序序列，如将空结点也加入前序序列，也可以唯一确定一棵树，这个性质可以应用到二叉树的序列化反序列化。



### 基本操作

* 简单[#104 Maximum Depth of Binary Tree](./#104 Maximum Depth of Binary Tree.md)    计算树的深度
* $\bigstar$困难[#124 Binary Tree Maximum Path Sum](./#124 Binary Tree Maximum Path Sum.md)    递归计算树的最大路径之和
* $\clubs$中等[#156 Binary Tree Upside Down](./#156 Binary Tree Upside Down.md)    上下翻转二叉树
* $\bigstar$中等[#222 Count Complete Tree Nodes](./#222 Count Complete Tree Nodes.md)    完全二叉树的二分查找实现结点统计
* $\bigstar$中等[#236 Lowest Common Ancestor of a Binary Tree](./#236 Lowest Common Ancestor of a Binary Tree.md)    二叉树寻找最近公共祖先
* $\clubs$中等[#250 Count Univalue Subtrees](./#250 Count Univalue Subtrees.md)    同值子树统计
* 中等[#298 Binary Tree Longest Consecutive Sequence](./#298 Binary Tree Longest Consecutive Sequence.md)    最大连续子序列长度
* $\bigstar$困难[#297 Serialize and Deserialize Binary Tree](./#297 Serialize and Deserialize Binary Tree.md)    前序序列变种序列化反序列化二叉树
* $\bigstar$中等[#337 House Robber III](./#337 House Robber III.md)    递归与动态规划结合

### 遍历

* $\clubs$中等[#94 Binary Tree Inorder Traversal](./#94 Binary Tree Inorder Traversal.md)    中序遍历
* 简单[#100 Same Tree](./#100 Same Tree.md)    遍历比较两个二叉树
* 简单[#101 Symmetric Tree](./#101 Symmetric Tree.md)    遍历比较镜像树，为[#100 Same Tree](./#100 Same Tree.md)的延续
* 中等[#102 Binary Tree Level Order Traversal](./#102 Binary Tree Level Order Traversal.md)    层次遍历的变形
* 中等[#103 Binary Tree Zigzag Level Order Traversal](./#103 Binary Tree Zigzag Level Order Traversal.md)    锯齿层次遍历，[#102 Binary Tree Level Order Traversal](./#102 Binary Tree Level Order Traversal.md)的延续
* $\bigstar$中等[#105 Construct Binary Tree from Preorder and Inorder Traversal](./#105 Construct Binary Tree from Preorder and Inorder Traversal.md)    从中序、前序遍历构建二叉树
* 中等[#106 Construct Binary Tree from Inorder and Postorder Traversal](./#106 Construct Binary Tree from Inorder and Postorder Traversal.md)    从中序遍历、后序遍历构造二叉树
* 简单[#107 Binary Tree Level Order Traversal II](./#107 Binary Tree Level Order Traversal II.md)    从底到顶的层次遍历，[#102 Binary Tree Level Order Traversal](./#102 Binary Tree Level Order Traversal.md)的延续
* $\clubs$简单[#111 Minimum Depth of Binary Tree](./#111 Minimum Depth of Binary Tree.md)    树的最小深度
* 简单[#112 Path Sum](./#112 Path Sum.md)    遍历计算路径之和
* $\bigstar$[#113 Path Sum II](./#113 Path Sum II.md)    遍历找出所有符合条件的路径
* $\clubs$中等[#114 Flatten Binary Tree to Linked List](./#114 Flatten Binary Tree to Linked List.md)    将树转换为前序序列的链表
* $\bigstar$中等[#116 Populating Next Right Pointers in Each Node](./#116 Populating Next Right Pointers in Each Node.md)    层次遍历及巧妙的结构应用
* $\bigstar$中等[#117 Populating Next Right Pointers in Each Node II](./#117 Populating Next Right Pointers in Each Node II.md)    [#116 Populating Next Right Pointers in Each Node](./#116 Populating Next Right Pointers in Each Node.md)的扩展
* 中等[#129 Sum Root to Leaf Numbers](./#129 Sum Root to Leaf Numbers.md)    二叉树遍历求和
* 简单[#144 Binary Tree Preorder Traversal](./#144 Binary Tree Preorder Traversal.md)    前序遍历
* 中等[#145 Binary Tree Postorder Traversal](./#145 Binary Tree Postorder Traversal.md)    后序遍历
* 中等[#199 Binary Tree Right Side View](./#199 Binary Tree Right Side View.md)   层次遍历实现树的右侧投影
* 简单[#226 Invert Binary Tree](./#226 Invert Binary Tree.md)    遍历反转二叉树
* 简单[#257 Binary Tree Paths](./#257 Binary Tree Paths.md)    前序遍历二叉树获得路径
* $\clubs$中等[#366 Find Leaves of Binary Tree](./#366 Find Leaves of Binary Tree.md)    后序遍历的变体
* 简单[#404 Sum of Left Leaves](./#404 Sum of Left Leaves.md)    前序遍历的应用
* $\clubs$中等[#437 Path Sum III](./#437 Path Sum III.md)    保存路径，深度遍历，向前计算路径和
* $\clubs$中等[#536 Construct Binary Tree from String](./#536 Construct Binary Tree from String.md)    根据前序序列的变形恢复二叉树
* 中等[#508 Most Frequent Subtree Sum](./#508 Most Frequent Subtree Sum.md)    前序遍历记录子树和

## 树与森林

&emsp;树的主要知识点在于树的表示。树可以表示为父子链表表示法、父子兄弟链表表示法、双亲表示法。但其中最通用的是父子兄弟链表表示法，因为可以将树表示为二叉树的形式；同理，森林也可以利用这种思想，将森林表示为二叉树的形式。

* 繁杂[#428 Serialize and Deserialize N-ary Tree](./#428 Serialize and Deserialize N-ary Tree.md)    N叉树的序列化和反序列化设计
* 中等[#429 N-ary Tree Level Order Traversal](./#429 N-ary Tree Level Order Traversal.md)    N叉树层次遍历
* 繁杂[#431 Encode N-ary Tree to Binary Tree](./#431 Encode N-ary Tree to Binary Tree.md)    前序遍历转化N叉树为父子兄弟链表表示的二叉树

## 树的应用

&emsp;题目较少，霍夫曼编码需要重点掌握。

### 计算表达式



### 霍夫曼树

