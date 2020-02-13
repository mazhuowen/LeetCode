[toc]

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.



Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.



## 题目解读

&emsp;给定二叉树，设计序列化与反序列化功能。

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
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

## 程序设计

* 对于二叉树的序列化可以使用前序序列，但单凭前序序列决定不了一棵二叉树。可以将空结点也前序遍历加入进去，这样就可以决定一棵二叉树了。
* 对于反序列化采用递归，从左至右遍历序列化链表，每个非`null`结点后的第一个值是左子节点，递归左子节点同时删除遍历的链表结点，这样左子树递归完成后就是右子树了，同理递归。

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null) {
            return "";
        }
        StringBuffer sb = new StringBuffer();
        Stack<Object> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            Object cur = stack.pop();
            // 当前为null，拼接并继续
            if(cur instanceof String) {
                sb.append(cur).append(",");
                continue;
            } 
            // 不为null，拼接并将子节点入栈（包含null）
            TreeNode node = (TreeNode)cur;
            sb.append(node.val).append(",");
            stack.push(node.right == null ? "null" : node.right);
            stack.push(node.left == null ? "null" : node.left);
        }
        // 截取最后一位逗号
        String res = sb.toString().substring(0, sb.length() - 1);
        return res;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data == null || data.isEmpty()) {
            return null;
        }
        String[] express = data.split(",");
        return deserialize(new LinkedList<>(Arrays.asList(express)));
    }

    // 递归前序序列的变形
    private TreeNode deserialize(List<String> express) {
        String sympol = express.get(0);
        express.remove(0);
        // 空结点，不再遍历
        if("null".equals(sympol)) {
            return null;
        } 
        // 创建结点，递归遍历
        else {
            TreeNode node = new TreeNode(Integer.valueOf(sympol));
            node.left = deserialize(express);
            node.right = deserialize(express);
            return node;
        }
    }
}
```

* 序列化的过程也可以使用递归实现。

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null) {
            return "";
        }
        StringBuffer sb = new StringBuffer();
        serialize(root, sb);
        // 去除第一个逗号
        return sb.toString().substring(1);
    }

    private void serialize(TreeNode root, StringBuffer sb) {
        // 空结点，拼接并停止遍历
        if(root == null) {
            sb.append(",").append("null");
            return;
        }
        // 拼接并继续前序遍历
        sb.append(",").append(root.val);
        serialize(root.left, sb);
        serialize(root.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data == null || data.isEmpty()) {
            return null;
        }
        String[] express = data.split(",");
        return deserialize(new LinkedList<>(Arrays.asList(express)));
    }

    // 递归前序序列的变形
    private TreeNode deserialize(List<String> express) {
        String sympol = express.get(0);
        express.remove(0);
        // 空结点，不再遍历
        if("null".equals(sympol)) {
            return null;
        } 
        // 创建结点，递归遍历
        else {
            TreeNode node = new TreeNode(Integer.valueOf(sympol));
            node.left = deserialize(express);
            node.right = deserialize(express);
            return node;
        }
    }
}
```

## 性能分析

&emsp;全递归两个方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：18ms，在所有java提交中击败了66.96%的用户。

内存消耗：50.5MB，在所有java提交中击败了5.09%的用户。

## 官方解题

&emsp;思路一致。

