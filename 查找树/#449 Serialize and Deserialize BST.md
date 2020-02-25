[toc]

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible**.

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.



## 题目解读

&emsp;设计二叉查找树序列化和反序列化方法。

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

* 题目要求序列化后的字符串要尽可能的压缩，可以使用前序序列，反序列化使用递归方法。

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuffer sb = new StringBuffer();
        if(root == null) {
            return sb.toString();
        }
        // 前序遍历
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            sb.append(cur.val).append(",");
            if(cur.right != null) {
                stack.push(cur.right);
            }
            if(cur.left != null) {
                stack.push(cur.left);
            }
        }
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data.isEmpty()) {
            return null;
        }
        String[] express = data.split(",");
        return transToTree(express, 0, express.length - 1);
    }

    private TreeNode transToTree(String[] express, int start, int end) {
        if(start > end) {
            return null;
        }
        TreeNode head = new TreeNode(Integer.parseInt(express[start]));
        int split = -1;
        // 寻找右子树
        for(int i = start + 1; i <= end; i++) {
            if(Integer.parseInt(express[i]) > Integer.parseInt(express[start])) {
                split = i;
                break;
            }
        }
        // 无右子树，递归
        if(split == -1) {
            head.left = transToTree(express, start + 1, end);
        } 
        // 有右子树，递归
        else {
            head.left = transToTree(express, start + 1, split - 1);
            head.right = transToTree(express, split, end);   
        }
        return head;
    }
}
```

* 注意到寻找右子树方法可以优化为二分查找，优化后：

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuffer sb = new StringBuffer();
        if(root == null) {
            return sb.toString();
        }
        // 前序遍历
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            sb.append(cur.val).append(",");
            if(cur.right != null) {
                stack.push(cur.right);
            }
            if(cur.left != null) {
                stack.push(cur.left);
            }
        }
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data.isEmpty()) {
            return null;
        }
        String[] express = data.split(",");
        return transToTree(express, 0, express.length - 1);
    }

    private TreeNode transToTree(String[] express, int start, int end) {
        if(start > end) {
            return null;
        }
        TreeNode head = new TreeNode(Integer.parseInt(express[start]));
        int split = binarySearch(express, head.val, start + 1, end);
        // 无右子树
        if(split == -1) {
            head.left = transToTree(express, start + 1, end);
        } else {
            head.left = transToTree(express, start + 1, split - 1);
            head.right = transToTree(express, split, end);   
        }
        return head;
    }
    // 查找第一个大于target的索引
    private int binarySearch(String[] express, int target, int start, int end) {
        while(start < end) {
            int mid = start + (end - start) / 2;
            if(Integer.parseInt(express[mid]) > target) {
                end = mid;
            } else {
                start = mid + 1;
            }
        }
        if(start > end || Integer.parseInt(express[start]) < target) {
            return -1;
        } else {
            return start;
        }
    }
}
```



## 性能分析

&emsp;序列化方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。反序列化方法时间复杂度最坏为左链表，此时$O(N^2)$，空间复杂度为$O(1)$。

执行用时：15ms，在所有java提交中击败了37.27%的用户。

内存消耗：42MB，在所有java提交中击败了5.33%的用户。

&emsp;二分查找优化后，反序列化最坏时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了74.31%的用户。

内存消耗：42MB，在所有java提交中击败了5.33%的用户。

## 官方解题

&emsp;官方提供的优化思路为将数字编码为字符，而不是字符串，同时也省去了分隔符，节省空间；其次反序列化递归不再查询区间，而是按顺序递归。

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuffer sb = new StringBuffer();
        serialize(root, sb);
        return sb.toString();
    }
    // 前序遍历
    private void serialize(TreeNode root, StringBuffer sb) {
        if(root == null) {
            return;
        }
        sb.append((char)root.val);
        serialize(root.left, sb);
        serialize(root.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        LinkedList<Character> list = new LinkedList<>();
        for(int i = 0; i < data.length(); i++) {
            list.add(data.charAt(i));
        }
        return deserialize(list, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }

    private TreeNode deserialize(LinkedList<Character> express, int low, int high) {
        // 结点超出当前范围，表示前一个根节点没有相应的子结点，返回空
        if(express.isEmpty() || express.getFirst() > high || express.getFirst() < low) {
            return null;
        }
        TreeNode head = new TreeNode((int)express.removeFirst());
        head.left = deserialize(express, low, head.val);
        head.right = deserialize(express, head.val, high);
        return head;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：3ms，在所有java提交中击败了98.84%的用户。

内存消耗：42MB，在所有java提交中击败了5.33%的用户。