[toc]

## 栈的基本操作

&emsp;栈的最基本特性就是先进后出，围绕着出栈、入栈操作，需要分析具体问题的规律，如[#71 Simplify Path](./#71 Simplify Path.md)中`../`对应返回父目录，意味着遇到`../`需要弹出栈顶元素；[#735 Asteroid Collision](./#735 Asteroid Collision.md)中行星的碰撞只能是栈顶值为正，当前待入栈值为负；通过分析这些特定的规律，得出栈的操作规则。栈最典型的应用抽象就是括号匹配，每一个闭括号必须对应相应的开括号，通过栈来判断匹配。括号匹配变形很多，如[#385 Mini Parser](./#385 Mini Parser.md)根据对象序列化后的开方括号来创建对象，闭方括号来维护对象嵌套关系；[#636 Exclusive Time of Functions](./#636 Exclusive Time of Functions.md)中方法的开始就是开括号，方法的结束是闭括号。

&emsp;除了基本应用，栈还有几个深入的应用。最经典应用就是计算器，任何计算表达式都可通过中缀表达式转后缀表达式、后缀表达式来计算求解；但具体问题具体对待，对于只有加减运算和括号的表达式，可以设置符号来整体的改变括号中的运算结果，转化为求和问题。对于没有括号的运算表达式，优先级只有操作符的优先级，可使用双栈运算。存在负号的计算表达式可以找到负号的规律，转化为普通计算表达式。

&emsp;栈还在柱状图面积计算中发挥着重要作用。在积水计算中，栈可以用来匹配两侧挡板，从而计算积水。直方图面积的计算是分治思想，栈利用结构特性简化了分治法。

&emsp;对于字典序类问题，需要仔细分析序列中变与不变的关系。二叉树利用栈较多的是二叉树的前序、中序、后序遍历，及给定两个遍历，恢复二叉树结构等。

* 简单[#20 Valid Parentheses](./#20 Valid Parentheses.md)    括号匹配，出栈、入栈基础操作
* $\clubs$困难[#32 Longest Valid Parentheses](./#32 Longest Valid Parentheses.md)    有效括号序列判断计数，多思路
* 简单[#71 Simplify Path](./#71 Simplify Path.md)    利用栈将Linux系统绝对路径化简
* $\clubs$简单[#155 Min Stack](./#155 Min Stack.md)    单链表实现改进的栈
* 简单[#232 Implement Queue using Stacks](./#232 Implement Queue using Stacks.md)    栈的基本操作
* 中等[#341 Flatten Nested List Iterator](./#341 Flatten Nested List Iterator.md)    反序遍历进行栈操作
* $\bigstar$繁杂[#385 Mini Parser](./#385 Mini Parser.md)    括号配对的变形
* 中等[#394 Decode String](./#394 Decode String.md)    括号配对变形
* 中等[#439 Ternary Expression Parser](./#439 Ternary Expression Parser.md)    反序遍历进行栈操作
* 简单[#581 Shortest Unsorted Continuous Subarray](./#581 Shortest Unsorted Continuous Subarray.md)    单调栈
* 繁杂[#591 Tag Validator](./#591 Tag Validator.md)    括号匹配的繁杂应用
* $\clubs$中等[#636 Exclusive Time of Functions](./#636 Exclusive Time of Functions.md)    括号匹配的抽象
* $\clubs$中等[#678 Valid Parenthesis String](./#678 Valid Parenthesis String.md)    带有通配符的括号匹配
* 简单[#682 Baseball Game](./#682 Baseball Game.md)    栈的基本操作
* 简单[#716 Max Stack](./#716 Max Stack.md)    辅助栈
* 繁杂[#726 Number of Atoms](./#726 Number of Atoms.md)    加入有序字典的栈基本操作
* $\clubs$中等[#735 Asteroid Collision](./#735 Asteroid Collision.md)    栈的匹配
* 简单[#844 Backspace String Compare](./#844 Backspace String Compare.md)    括号匹配变形
* 中等[#856 Score of Parentheses](./#856 Score of Parentheses.md)    括号匹配变形
* $\clubs$技巧[#880 Decoded String at Index](./#880 Decoded String at Index.md)    理解规律逆向操作
* 简单[#921 Minimum Add to Make Parentheses Valid](./#921 Minimum Add to Make Parentheses Valid.md)    括号计数消除
* 简单[#1021 Remove Outermost Parentheses](./#1021 Remove Outermost Parentheses.md)    通过计数实现栈的思想
* 中等[#1190 Reverse Substrings Between Each Pair of Parentheses](./#1190 Reverse Substrings Between Each Pair of Parentheses.md)    栈保存括号索引

## 栈的应用

### 柱状图

* $\bigstar$困难[#42 Trapping Rain Water](./#42 Trapping Rain Water.md)    积水计算，涉及双指针、栈
* $\bigstar$困难[#84 Largest Rectangle in Histogram](./#84 Largest Rectangle in Histogram.md)    直方图最大矩形，分治法、栈实现的分治思想
* $\bigstar$困难[#85 Maximal Rectangle](./#85 Maximal Rectangle.md)    转化为#84

### 计算器

* $\clubs$简单[#150 Evaluate Reverse Polish Notation](./#150 Evaluate Reverse Polish Notation.md)    后缀表达式计算
* $\clubs$中等[#224 Basic Calculator](./#224 Basic Calculator.md)    只有加减及括号的表达式计算
* 中等[#227 Basic Calculator II](./#227 Basic Calculator II.md)    没有括号的表达式计算
* 繁杂[#770 Basic Calculator IV](./#770 Basic Calculator IV.md)    存在变量的计算问题，创建多项式对象
* 困难[#772 Basic Calculator III](./#772 Basic Calculator III.md)    存在负号的计算问题

### 字典序

* $\bigstar$困难[#316 Remove Duplicate Letters](./#316 Remove Duplicate Letters.md)    字典序
* $\clubs$中等[#402 Remove K Digits](./#402 Remove K Digits.md)    栈的贪婪算法
* $\clubs$中等[#456 132 Pattern](./#456 132 Pattern.md)    技巧
* 简单[#496 Next Greater Element I](./#496 Next Greater Element I.md)    字典保存较大后继
* 简单[#503 Next Greater Element II](./#503 Next Greater Element II.md)    [#496 Next Greater Element I](./#496 Next Greater Element I.md)的延续
* 简单[#739 Daily Temperatures](./#739 Daily Temperatures.md)    遍历遇到较大值则出栈计算
* $\clubs$困难[#768 Max Chunks To Make Sorted II](./#768 Max Chunks To Make Sorted II.md)    单调栈记录分块的最大值
* 中等[#1019 Next Greater Node In Linked List](./#1019 Next Greater Node In Linked List.md)    单调栈
* $\clubs$困难[#1063 Number of Valid Subarrays](./#1063 Number of Valid Subarrays.md)    单调栈
* 中等[#1081 Smallest Subsequence of Distinct Characters](./#1081 Smallest Subsequence of Distinct Characters.md)    [#316 Remove Duplicate Letters](./#316 Remove Duplicate Letters.md)的变形

### 二叉树

* 中等[#331 Verify Preorder Serialization of a Binary Tree](./#331 Verify Preorder Serialization of a Binary Tree.md)    检测变形的前序序列是否有效