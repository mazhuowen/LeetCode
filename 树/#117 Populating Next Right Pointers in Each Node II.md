[toc]

Given a binary tree

```c++
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```


Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

 

**Follow up**:

* You may only use constant extra space.
* Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

Constraints:

* The number of nodes in the given tree is less than 6000.
* $-100 \le \text{node.val} \le 100$



## 题目解读

&emsp;给定二叉树，同一层的结点连接其后继结点。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/
class Solution {
    public Node connect(Node root) {
        
    }
}
```

## 程序设计

* 空间复杂度不是常量级别的方法，层次遍历可以很好的解决。这儿只考虑常量界别的方法。参考[#116 Populating Next Right Pointers in Each Node](./#116 Populating Next Right Pointers in Each Node.md)中的思路，在本题中情况更加复杂，首先满二叉树发起操作的结点都在左支路，也就是最左边的边界，对于一般的二叉树需要确定最左边的边界；最左边的边界应该是：当存在左子节点则遍历左子节点，不存在则遍历右子节点，若右子节点不存在，则遍历`next`结点然后重复上述操作，这样就确定了每一层第一个发起`next`操作的结点。
* 对于`next`操作，如果存在两个子节点，则左子节点指向右子节点，右子节点指向后继结点；若存在一个子节点，则这个子节点指向后继结点；若不存在子节点则遍历后继结点。
* 后继结点的父结点就是当前父结点向后遍历遇到的第一个有子节点的结点，然后根据子节点数目判断左儿子还是右儿子是后继结点。得到后继结点后连接即可。

```java
class Solution {
    public Node connect(Node root) {
        if(root == null) {
            return null;
        }
        Node traversal = root;
        // 遍历树的最左侧边界
        while(traversal.left != null || traversal.right != null || traversal.next != null) {
            // 当前边界已经判断完，遍历next找到存在子节点的点继续遍历，向右遍历
            if(traversal.left == null && traversal.right == null) {
                traversal = traversal.next;
                continue;
            } 
            Node parent = traversal;
            while(parent != null) {
                // 没有子节点则继续遍历
                if(parent.left == null && parent.right == null) {
                    parent = parent.next;
                    continue;
                }
                // 存在两个结点，则左子节点指向右子节点
                if(parent.left != null && parent.right != null) {
                    parent.left.next = parent.right;
                } 
                // 存在一个左子结点或存在一个右子节点或存在两个结点中的右子节点
                // 指向下一个存在子节点的父结点的子节点
                Node pre = parent.right == null ? parent.left : parent.right;
                Node cur = parent.next;
                while(cur != null) {
                    // 找到parent后存在子节点的结点
                    if(cur.left != null || cur.right != null) {
                        break;
                    }
                    cur = cur.next;
                }
                if(cur != null) {
                    Node next = cur.left == null ? cur.right : cur.left;
                    // 链接
                    pre.next = next;
                } 
                // 继续遍历
                parent = cur;
            }
            // 向下遍历
            traversal = traversal.left == null ? traversal.right : traversal.left;
        }
        return root;
    }
}
```

## 性能分析

&emsp;时间复杂度的为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了67.74%的用户。

内存消耗：47MB，在所有java提交中击败了41.90%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有参考层次遍历但不实用队列，非常巧妙的使用哑结点串联同层结点的解法，空间复杂度是$O(1)$。

```java
class Solution {
    public static Node connect(Node root) {
        // 同层遍历起始结点
        Node traversal = root;
        while (traversal != null){
            // 哑结点连接同层结点
            Node dummy = new Node();
            Node trail = dummy;
            // 遍历同层，连接下一层
            while (traversal != null){
                if(traversal.left != null){
                    trail.next = traversal.left;
                    trail = trail.next;
                }
                if(traversal.right != null){
                    trail.next = traversal.right;
                    trail = trail.next;
                }
                // 向右遍历
                traversal = traversal.next;
            }
            // 遍历结束，重定向为下一层第一个结点，继续遍历
            traversal = dummy.next;
        }
        return root;
    }
}
```

社区中还有递归解法：

```java
class Solution {
    public Node connect(Node root) {
        // 空或叶节点返回
    	if(root == null || (root.left == null && root.right == null)) {
    		return root;
    	}
        // 需要拼接的最后一个结点
    	Node lastNode=null;
    	if(root.right == null) {
    		lastNode=root.left;
    	}else {
    		if(root.left != null) {
    			root.left.next=root.right;
    		}
    		lastNode=root.right;
    	}
        // 查找后面存在子节点的结点，其子节点就是需要连接的结点
        Node indexNode=root.next;
    	while (indexNode!=null) {
			if(indexNode.left == null && indexNode.right == null) {
				indexNode=indexNode.next;
			}
			else if(indexNode.left!=null) {
				lastNode.next=indexNode.left;
				break;
			}else {
				lastNode.next=indexNode.right;
				break;
			}
		}
    	// 递归
    	connect(root.right);
    	connect(root.left);
    	return root;
    }
}
```