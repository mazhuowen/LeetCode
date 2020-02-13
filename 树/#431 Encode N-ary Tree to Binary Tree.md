[toc]

Design an algorithm to encode an N-ary tree into a binary tree and decode the binary tree to get the original N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. Similarly, a binary tree is a rooted tree in which each node has no more than 2 children. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that an N-ary tree can be encoded to a binary tree and this binary tree can be decoded to the original N-nary tree structure.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See following example).



Constraints:

* The height of the n-ary tree is less than or equal to 1000
* The total number of nodes is between $[0, 10^4]$
* Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.



## 题目解读

&emsp;设计算法将N叉树转换为二叉树，同时可以将二哈书还原为N叉树。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Codec {

    // Encodes an n-ary tree to a binary tree.
    public TreeNode encode(Node root) {
        
    }

    // Decodes your binary tree to an n-ary tree.
    public Node decode(TreeNode root) {
        
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(root));
```

## 程序设计

* 转换为二叉树就是将树表示为父子兄弟链表表示法，还原是其逆过程。二叉树左结点表示儿子，右结点表示兄弟。使用前序遍历来同时遍历维护两个树。

```java
class Codec {

    // Encodes an n-ary tree to a binary tree.
    public TreeNode encode(Node root) {
        if(root == null) {
            return null;
        }
        // 遍历n叉树
        Stack<Node> stack = new Stack<>();
        // 协同维护二叉树关系
        Stack<TreeNode> bt = new Stack<>();
        stack.push(root);
        TreeNode head = new TreeNode(root.val);
        bt.push(head);
        // 前序遍历
        while(!stack.isEmpty()) {
            Node cur = stack.pop();
            TreeNode temp = bt.pop();
            // 有子节点
            List<Node> children = cur.children;
            if(children != null && children.size() > 0) {
                // 大儿子为父结点的左结点，入栈
                Node bro = children.get(0);
                TreeNode tempBro = new TreeNode(bro.val);
                // 维护关系
                temp.left = tempBro;
                stack.push(bro);
                bt.push(tempBro);
                // 其它儿子结点为大儿子的右结点
                for(int i = 1; i < children.size(); i++) {
                    tempBro.right = new TreeNode(children.get(i).val);
                    tempBro = tempBro.right;
                    // 入栈
                    stack.push(children.get(i));
                    bt.push(tempBro);
                }
            }
        }
        return head;
    }

    // Decodes your binary tree to an n-ary tree.
    public Node decode(TreeNode root) {
        if(root == null) {
            return null;
        }
        Node head = new Node(root.val, new LinkedList<>());
        // 遍历二叉树
        Stack<TreeNode> stack = new Stack<>();
        Stack<Node> nt = new Stack<>();
        stack.push(root);
        nt.push(head);
        // 前序遍历
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            Node tempP = nt.pop();
            // 表示n树有子节点，遍历二叉树的左子节点及左子节点的右链路
            if(cur.left != null) {
                TreeNode son = cur.left;
                while(son != null) {
                    // 维护关系
                    Node tempS = new Node(son.val, new LinkedList<>()); 
                    tempP.children.add(tempS);
                    // 入栈
                    stack.push(son);
                    nt.push(tempS);
                    // 迭代
                    son = son.right;
                }
            }
        }
        return head;
    }
}
```

* 由于主体是前序遍历，可使用递归实现。

```java
class Codec {

    // Encodes an n-ary tree to a binary tree.
    public TreeNode encode(Node root) {
        if(root == null) {
            return null;
        }
        TreeNode parent = new TreeNode(root.val);
        List<Node> children = root.children;
        // 递归子节点
        if(children != null && children.size() > 0) {
            TreeNode son = encode(children.get(0));
            // 大儿子为左结点
            parent.left = son;
            // 其它儿子为大儿子右继
            for(int i = 1; i < children.size(); i++) {
                son.right = encode(children.get(i));
                son = son.right;
            }
        }
        return parent;
    }

    // Decodes your binary tree to an n-ary tree.
    public Node decode(TreeNode root) {
        if(root == null) {
            return null;
        }
        Node parent = new Node(root.val, new LinkedList<>());
        // 有子节点
        if(root.left != null) {
            // 遍历左子节点的右链路
            TreeNode son = root.left;
            while(son != null) {
                // 递归
                parent.children.add(decode(son));
                son = son.right;
            }
        }
        return parent;
    }
}
```

## 性能分析

&emsp;非递归思路两个方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了8.00%的用户。

内存消耗：51.2MB，在所有java提交中击败了21.62%的用户。

&emsp;递归思路两个方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了98.00%的用户。

内存消耗：51.8MB，在所有java提交中击败了21.62%的用户。

## 官方解题

&emsp;暂无，密切关注。