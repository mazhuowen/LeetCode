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
* 中等[#654 Maximum Binary Tree](./#654 Maximum Binary Tree.md)    递归创建树
* $\clubs$中等[#958 Check Completeness of a Binary Tree](./#958 Check Completeness of a Binary Tree.md)    判断是否是完全二叉树

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
* $\clubs$中等[#314 Binary Tree Vertical Order Traversal](./#314 Binary Tree Vertical Order Traversal.md)    层次遍历实现“垂直遍历”
* $\clubs$中等[#366 Find Leaves of Binary Tree](./#366 Find Leaves of Binary Tree.md)    后序遍历的变体
* 简单[#404 Sum of Left Leaves](./#404 Sum of Left Leaves.md)    前序遍历的应用
* $\clubs$中等[#437 Path Sum III](./#437 Path Sum III.md)    保存路径，深度遍历，向前计算路径和
* 中等[#508 Most Frequent Subtree Sum](./#508 Most Frequent Subtree Sum.md)    前序遍历记录子树和
* 简单[#513 Find Bottom Left Tree Value](./#513 Find Bottom Left Tree Value.md)    中序遍历或前序遍历记录最大层
* 简单[#515 Find Largest Value in Each Tree Row](./#515 Find Largest Value in Each Tree Row.md)    中序遍历
* $\clubs$中等[#536 Construct Binary Tree from String](./#536 Construct Binary Tree from String.md)    根据前序序列的变形恢复二叉树
* 简单[#543 Diameter of Binary Tree](./#543 Diameter of Binary Tree.md)    计算路径和
* $\clubs$中等[#545 Boundary of Binary Tree](./#545 Boundary of Binary Tree.md)    前序遍历的变形结合后序遍历实现边界遍历
* $\clubs$中等[#549 Binary Tree Longest Consecutive Sequence II](./#549 Binary Tree Longest Consecutive Sequence II.md)    树的路径序列
* 简单[#572 Subtree of Another Tree.md](./#572 Subtree of Another Tree.md)    二叉树遍历判断子结构
* 简单[#617 Merge Two Binary Trees](./#617 Merge Two Binary Trees.md)    前序遍历
* 中等[#623 Add One Row to Tree](./#623 Add One Row to Tree.md)    树的遍历
* $\clubs$中等[#652 Find Duplicate Subtrees](./#652 Find Duplicate Subtrees.md)    前序遍历字典记录子树
* 中等[#655 Print Binary Tree](./#655 Print Binary Tree.md)    将二叉树打印为列表形式
* 中等[#662 Maximum Width of Binary Tree](./#662 Maximum Width of Binary Tree.md)    树的数组索引与层次遍历的结合
* 中等[#663 Equal Tree Partition](./#663 Equal Tree Partition.md)    树的遍历求和
* $\clubs$中等[#742 Closest Leaf in a Binary Tree](./#742 Closest Leaf in a Binary Tree.md)    后序遍历记录状态
* 中等[#863 All Nodes Distance K in Binary Tree](./#863 All Nodes Distance K in Binary Tree.md)    [#742 Closest Leaf in a Binary Tree](./#742 Closest Leaf in a Binary Tree.md)一般化的题目
* $\clubs$中等[#889 Construct Binary Tree from Preorder and Postorder Traversal](./#889 Construct Binary Tree from Preorder and Postorder Traversal.md)    根据前序序列和后序序列构建一棵树
* $\clubs$中等[#968 Binary Tree Cameras](./#968 Binary Tree Cameras.md)    前序遍历巧妙返回状态
* $\clubs$中等[#979 Distribute Coins in Binary Tree](./#979 Distribute Coins in Binary Tree.md)    后序遍历返回额外信息
* 中等[#987 Vertical Order Traversal of a Binary Tree](./#987 Vertical Order Traversal of a Binary Tree.md)    [#314 Binary Tree Vertical Order Traversal](./#314 Binary Tree Vertical Order Traversal.md)的进阶，增加同增顺序要求
* 简单[#993 Cousins in Binary Tree](./#993 Cousins in Binary Tree.md)    层次遍历
* $\clubs$中等[#1104 Path In Zigzag Labelled Binary Tree](./#1104 Path In Zigzag Labelled Binary Tree.md)    树的数组索引表示
* $\clubs$中等[#1110 Delete Nodes And Return Forest](./#1110 Delete Nodes And Return Forest.md)    后序遍历返回结点是否删除
* 简单[#1120 Maximum Average Subtree](./#1120 Maximum Average Subtree.md)    后序遍历计算
* 中等[#1145 Binary Tree Coloring Game](./#1145 Binary Tree Coloring Game.md)    发现规律，遍历统计左右子树
* 中等[#1161 Maximum Level Sum of a Binary Tree](./#1161 Maximum Level Sum of a Binary Tree.md)    层次遍历
* 中等[#1257 Smallest Common Region](./#1257 Smallest Common Region.md)    最近公共父节点问题
* 中等[#1372 Longest ZigZag Path in a Binary Tree](./#1372 Longest ZigZag Path in a Binary Tree.md)    前序遍历统计交替路径
* $\clubs$中等[#面试题04.12 Paths with Sum LCCI](./#面试题04.12 Paths with Sum LCCI.md)    数组记录路径和

## 树与森林

&emsp;树的主要知识点在于树的表示。树可以表示为父子链表表示法、父子兄弟链表表示法、双亲表示法。但其中最通用的是父子兄弟链表表示法，因为可以将树表示为二叉树的形式；同理，森林也可以利用这种思想，将森林表示为二叉树的形式。

* 繁杂[#428 Serialize and Deserialize N-ary Tree](./#428 Serialize and Deserialize N-ary Tree.md)    N叉树的序列化和反序列化设计
* 中等[#429 N-ary Tree Level Order Traversal](./#429 N-ary Tree Level Order Traversal.md)    N叉树层次遍历
* 繁杂[#431 Encode N-ary Tree to Binary Tree](./#431 Encode N-ary Tree to Binary Tree.md)    前序遍历转化N叉树为父子兄弟链表表示的二叉树
* $\bigstar$困难[#440 K-th Smallest in Lexicographical Order](./#440 K-th Smallest in Lexicographical Order.md)    N叉树的抽象
* 简单[#589 N-ary Tree Preorder Traversal](./#589 N-ary Tree Preorder Traversal.md)    前序遍历
* $\bigstar$困难[#834 Sum of Distances in Tree](./#834 Sum of Distances in Tree.md)    通过指定根将无向图转化为树，通过根节点和子树关系计算路径和

## 树的应用

&emsp;题目较少，霍夫曼编码需要重点掌握。

### 计算表达式



### 霍夫曼树

