[toc]

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize an N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that an N-ary tree can be serialized to a string and this string can be deserialized to the original tree structure.



Constraints:

* The height of the n-ary tree is less than or equal to 1000
* The total number of nodes is between $[0, 10^4]$
* Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.



## 题目解读

&emsp;设计N叉树的序列化格式并设计反序列化方法。

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
class Codec {

    // Encodes a tree to a single string.
    public String serialize(Node root) {
        
    }

    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

## 程序设计

* 根据给定的数据结构，可知占用空间最小的序列化方案为``[1 [3[5 6] 2 4]]``，即嵌套表示。对于序列化，就是前序遍历的过程；对于反序列化，就是栈的括号匹配问题。
* 序列化将树转化为`[ 1 [ 3 [ 5 6 ] 2 4 ] ]`这种形式，每个符号之间用空格隔开，括号间的数值表示前一数值的子节点。使用树的前序遍历来拼接实现。
* 反序列化就是括号匹配的变形，遍历序列化字符串，数字开括号则入栈，闭括号则将开括号之间的结点弹出并赋值为栈顶结点的子节点。由于序列化字符串总是以`[]`开头结尾，可引入哑结点化解特殊情况。

```java
class Codec {

    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if(root == null) {
            return "[ ]";
        }
        StringBuffer sb = new StringBuffer("[");
        Stack<Object> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            Object temp = stack.pop();
            if(temp instanceof String) {
                sb.append(" ").append(temp);
                continue;
            }
            Node cur = (Node)temp;
            sb.append(" ").append(cur.val);
            // 判断子节点
            List<Node> children = cur.children;
            if(children != null && children.size() > 0) {
                // 有子节点，拼接开括号
                sb.append(" ").append("[");
                // 入栈一个标识结点，表示子节点完成，需要拼接闭括号
                stack.push("]");
                // 反序入栈
                for(int i = children.size() - 1; i >= 0; i--) {
                    stack.push(children.get(i));
                }
            }

        }
        sb.append(" ]");
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        // 哑结点
        Node dummy = new Node(-1);
        // 根据空格分割
        String[] express = data.split(" ");
        Stack<Object> satck = new Stack<>();
        satck.push(dummy);
        for(String sympol : express) {
            switch(sympol) {
                // 表示子节点，入栈
                case "[":
                    satck.push("[");
                    break;
                // 子节点结束，出栈
                case "]":
                    List<Node> l = new LinkedList<>();
                    while(!satck.isEmpty() && !"[".equals(satck.peek())) {
                        Node temp = (Node)satck.pop();
                        l.add(temp);
                    }
                    // 删除开括号
                    satck.pop();
                    // 由于栈的关系，需要翻转
                    Collections.reverse(l);
                    // 赋值
                    Node parent = (Node)satck.peek();
                    parent.children = l;
                    break;
                // Node对象
                default:
                    // 注意，题目没有说明，但要求即使没有子节点children必须是空串，不能为null
                    satck.push(new Node(Integer.valueOf(sympol), new LinkedList<>()));
                    break;
            }
        }
        Node root = null;
        if(dummy.children != null && dummy.children.size() >= 1) {
            root = dummy.children.get(0);
        }
        return root;
    }
}
```

> 题目没有说明，但是从结果看一个Node对象children不能为空，没有子节点必须设为空串，否则题目的验证代码报错。

## 性能分析

&emsp;序列化时间复杂度为$O(N)$，空间复杂度为$O(N)$。反序列化时间复杂度为$O(N)$，空间的复杂度为$O(N)$。

执行用时：18ms，在所有java提交中击败了18.84%的用户。

内存消耗：51.2MB，在所有java提交中击败了24.39%的用户。

## 官方解题

&emsp;暂无，密切关注。