[toc]

Given a binary tree, return all duplicate subtrees. For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are duplicate if they have the same structure with same node values.



## 题目解读

&emsp;找到树中重复的子树，并返回根节点。

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
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {

    }
}
```

## 程序设计

* 通过前序遍历保存所有遍历到的子树到字典，遍历时检查是否有重复。为了唯一标识一棵树，键值采用前序序列的变形。

```java
class Solution {
    private Map<String, TreeNode> record;
    private Set<TreeNode> set;

    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        record = new HashMap<>();
        set = new HashSet<>();
        if(root == null) {
            return new LinkedList<>();
        }
        preorder(root);
        List<TreeNode> res = new LinkedList<>(set);
        return res;
    }

    private String preorder(TreeNode root) {
        // 拼接空结点，这样可以唯一标识一棵树
        if(root == null) {
            return "null";
        }
        StringBuffer sb = new StringBuffer();
        sb.append("#").append(root.val);
        sb.append("#").append(preorder(root.left));
        sb.append("#").append(preorder(root.right));
        // 找到重复子树
        if(record.get(sb.toString()) != null) {
            set.add(record.get(sb.toString()));
        } 
        // 第一次遍历，加入字典
        else {
            record.put(sb.toString(), root);
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;前序遍历总的时间复杂度为$O(N)$；空间复杂度为$O(N)$。如果考虑字符串拼接等，则时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：28ms，在所有java提交中击败了36.26%的用户。

内存消耗：60.2MB，在所有java提交中击败了22.70%的用户。

## 官方解题

&emsp;官方将前序遍历的字典保存子树计数而不是子树根节点，也无需保存子树根节点。其次官方还提出了一种通过给每个子树设定`uuid`的方式：

```java
class Solution {
    int t;
    Map<String, Integer> trees;
    Map<Integer, Integer> count;
    List<TreeNode> ans;

    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        t = 1;
        trees = new HashMap();
        count = new HashMap();
        ans = new ArrayList();
        lookup(root);
        return ans;
    }

    public int lookup(TreeNode node) {
        if (node == null) return 0;
        String serial = node.val + "," + lookup(node.left) + "," + lookup(node.right);
        // 避免字符串拼接，设定子树id
        int uid = trees.computeIfAbsent(serial, x-> t++);
        count.put(uid, count.getOrDefault(uid, 0) + 1);
        if (count.get(uid) == 2)
            ans.add(node);
        return uid;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：18ms，在所有java提交中击败了64.99%的用户。

内存消耗：43.2MB，在所有java提交中击败了90.34%的用户。