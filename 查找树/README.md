[toc]

## 二叉查找树

&emsp;二叉查找树的 基本操作主要有查询、插入、删除，都可用递归和迭代方式实现；其中删除操作当待删除结点有两个子节点时还需要寻找替换结点删除。除了上述操作，二叉查找树最重要的性质是中序序列是增序的，可以利用这个性质实现很多功能。

* $\bigstar$中等[#95 Unique Binary Search Trees II](./#95 Unique Binary Search Trees II.md)    查找树的递归生成
* 中等[#96 Unique Binary Search Trees](./#96 Unique Binary Search Trees.md)    动态规划及数学知识的应用
* 中等[#98 Validate Binary Search Tree](./#98 Validate Binary Search Tree.md)    二叉查找树的中序序列性质
* $\bigstar$中等[#99 Recover Binary Search Tree](./#99 Recover Binary Search Tree.md)    中序遍历的应用
* 简单[#108 Convert Sorted Array to Binary Search Tree](./#108 Convert Sorted Array to Binary Search Tree.md)    转化二叉搜索树
* $\clubs$中等[#173 Binary Search Tree Iterator](./#173 Binary Search Tree Iterator.md)    设计二叉查找树的迭代功能
* 中等[#230 Kth Smallest Element in a BST](./#230 Kth Smallest Element in a BST.md)    二叉查找树中序序列性质应用
* 简单[#235 Lowest Common Ancestor of a Binary Search Tree](./#235 Lowest Common Ancestor of a Binary Search Tree.md)    最近公共祖先结点，搜索问题的变形
* $\clubs$中等[#255 Verify Preorder Sequence in Binary Search Tree](./#255 Verify Preorder Sequence in Binary Search Tree.md)    二叉查找树前序序列校验
* 简单[#270 Closest Binary Search Tree Value](./#270 Closest Binary Search Tree Value.md)    二叉查找树的搜索问题
* $\bigstar$困难[#272 Closest Binary Search Tree Value II](./#272 Closest Binary Search Tree Value II.md)    平衡二叉查找树前驱后继查找
* $\clubs$中等[#285 Inorder Successor in BST](./#285 Inorder Successor in BST.md)    搜索的方式查找后继结点
* 中等[#333 Largest BST Subtree](./#333 Largest BST Subtree.md)    检测二叉树中的二叉查找子树
* $\clubs$中等[#426 Convert Binary Search Tree to Sorted Doubly Linked List](./#426 Convert Binary Search Tree to Sorted Doubly Linked List.md)    二叉查找树转链表
* $\clubs$中等[#449 Serialize and Deserialize BST](./#449 Serialize and Deserialize BST.md)    前序序列递归生成二叉查找树
* 中等[#450 Delete Node in a BST](./#450 Delete Node in a BST.md)    二叉查找树的删除
* $\clubs$中等[#501 Find Mode in Binary Search Tree](./#501 Find Mode in Binary Search Tree.md)    求存在重复值二叉查找树中的众数
* 中等[#510 Inorder Successor in BST II](./#510 Inorder Successor in BST II.md)    寻找后继
* 简单[#530 Minimum Absolute Difference in BST](./#530 Minimum Absolute Difference in BST.md)    中序遍历求相邻元素差
* 简单[#538 Convert BST to Greater Tree](./#538 Convert BST to Greater Tree.md)    反序中序遍历
* 简单[#653 Two Sum IV - Input is a BST](./#653 Two Sum IV - Input is a BST.md)    中序遍历有序数组的两数之和
* 简单[#700 Search in a Binary Search Tree](./#700 Search in a Binary Search Tree.md)    二叉查找树的搜索
* 简单[#701 Insert into a Binary Search Tree](./#701 Insert into a Binary Search Tree.md)    二叉查找树的插入
* $\clubs$中等[#1214 Two Sum BSTs](./#1214 Two Sum BSTs.md)    两棵树遍历查找
* 简单[#1305 All Elements in Two Binary Search Trees](./#1305 All Elements in Two Binary Search Trees.md)    中序遍历

## AVL树

&emsp;平衡二叉树每个结点都需要维护一个属性记录高度，左右子树的高度之差不能超过2。若插入或删除打破了平衡，则需要单旋转和双旋转恢复平衡。

* 简单[#110 Balanced Binary Tree](./#110 Balanced Binary Tree.md)    后序遍历计算高度并比对

## 红黑树

&emsp;规定根节点到每个空结点路径上的黑结点数目一致，红结点的子节点不能是红结点。每个结点需要一个属性保存是红结点还是黑结点。若插入删除打破了平衡，则使用单旋转和双旋转恢复，这个过程还有重新着色的操作。

## AA树

&emsp;红黑树操作复杂，AA树通过规定左子节点层数必须小于父节点，右子节点层数可以等于父节点，而对应到红黑树，红结点层数和父节点相同，黑结点层数必须低于父节点。操作仍然是单旋转和双旋转及层数操作。

## 伸展树

&emsp;伸展树是为了频繁搜索的元素而发展的一种查找树，操作是变形的单旋转和双旋转。

## 线段树

&emsp;对于一些区间问题，可以引入线段树，将二叉查找树的搜索操作变为区间的包含关系。线段树是一种非常灵活的数据结构，它可以用于解决多种范围查询问题，比如在对数时间内从数组中找到最小值、最大值、总和、最大公约数、最小公倍数等。

&emsp;线段树的形式主要有四类，在已知叶节点（最小区间）的情况下通过数组构建完全二叉树、通过树对象构建二叉树，都属于静态线段树；另一类是动态线段树，如虽然已知叶节点，但是叶节点与次序有关，需要依次加入，此类的实现有基于数组的延迟传播和基于树对象的动态创建。

* $\bigstar$中等[#307 Range Sum Query - Mutable](./#307 Range Sum Query - Mutable.md)    线段树区间和统计
* $\bigstar$困难[#308 Range Sum Query 2D - Mutable](./#308 Range Sum Query 2D - Mutable.md)    二维数组的线段树区间统计
* $\bigstar$中等[#673 Number of Longest Increasing Subsequence](./#673 Number of Longest Increasing Subsequence.md)    动态线段树
* $\bigstar$繁杂[#699 Falling Squares](./#699 Falling Squares.md)    动态数组线段树延迟传播
* $\bigstar$困难[#850 Rectangle Area II](./#850 Rectangle Area II.md)    线段树的动态创建修改
* $\bigstar$困难[#1157 Online Majority Element In Subarray](./#1157 Online Majority Element In Subarray.md)    保存数字出现索引使用线段树指导统计

## B和B+树

&emsp;对于文件和索引系统，减少磁盘访问是关键，B和B+树就是典型代表。